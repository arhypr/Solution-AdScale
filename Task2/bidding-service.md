# Спецификация сервиса ставок (Bidding Service)

## 1. Границы сервиса

Сервис ставок отвечает за:

- Приём **bid requests** от Ad Server (внутренний вызов) или напрямую от DSP (через API Gateway).
- Проведение аукциона в реальном времени (взвешивание ставок, применение бизнес-правил, выбор победителя).
- Формирование **bid response** с ценой, идентификатором креатива, метаданными.
- Отправку метрик аукциона (win/loss, цена, latency) в Kafka для аналитики и финансов.
- Не занимается:
    - Формированием HTML/JS-разметки (это задача Delivery Service).
    - Записью статистики показов/кликов (только отправка событий в Kafka).
    - Финансовыми операциями (списание баланса выполняется Financial Service асинхронно по событиям win).

## 2. Зависимости от других сервисов

- **Ad Server** — вызывает Bidding Service по gRPC, передавая контекст пользователя, устройство, таргетинговые
  параметры.
- **Redis Cluster** — кэш для быстрого доступа к данным кампаний, ставок, таргетингов (обновляется асинхронно через
  события из Kafka).
- **PostgreSQL (primary)** — источник данных для кампаний и ставок, используется только для записи (CRUD через
  Advertiser Dashboard) и для перезагрузки кэша.
- **Financial Service** — не вызывается синхронно, но через Kafka получает события о выигранных аукционах для списания
  средств.
- **Kafka** — отправка событий аукциона (bid_win, bid_loss, bid_request_metrics) для потоковой обработки.
- **API Gateway** — для внешних DSP-запросов (аутентифицированных) маршрутизирует запросы на Bidding Service.

## 3. API сервиса

### 3.1 Внутреннее API (gRPC)

```protobuf
service BiddingService {
  rpc PlaceBid (BidRequest) returns (BidResponse);
}

message BidRequest {
  string request_id = 1;
  string user_id = 2;
  string device_id = 3;
  string ip = 4;
  string user_agent = 5;
  string placement_id = 6;      // идентификатор места показа
  repeated string site_categories = 7;
  map<string, string> targeting = 8;
  int64 timestamp = 9;           // время получения запроса
}

message BidResponse {
  string request_id = 1;
  bool win = 2;
  double price = 3;             // цена ставки
  string creative_id = 4;
  string campaign_id = 5;
  string ad_tag = 6;            // идентификатор креатива для Delivery
  int64 processing_time_ms = 7; // время обработки
}