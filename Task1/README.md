# Задание 1. Архитектурная диагностика и целевое видение

**Автор:** arhypr
**Дата:** 2026-08-29
**Репозиторий:** [Solution-AdScale](https://github.com/arhypr/Solution-AdScale)

## Краткое описание
В рамках задания проведён анализ текущей архитектуры AdScale (AS-IS), выявлены критические узкие места и риски, сформулированы архитектурные драйверы (функциональные требования, атрибуты качества, ограничения). Обоснован выбор стратегии эволюции — **Strangler Fig Pattern** с постепенной декомпозицией монолита. Разработано целевое видение (TO-BE) на горизонт 1 год, включая микросервисную архитектуру, событийно-ориентированную обработку, горизонтальное масштабирование и мультирегиональное развёртывание. Приняты ключевые архитектурные решения (ADR).

## Состав артефактов
| Файл | Описание |
|------|----------|
| [AS-IS.md](AS-IS.md) | Детальный анализ текущей архитектуры, выявленные проблемы и риски |
| [Drivers.md](./Drivers.md) | Функциональные требования, атрибуты качества (с приоритетами), ограничения |
| [TO-BE.md](./TO-BE.md) | Описание целевой архитектуры через год, принципы и ключевые изменения |

| [Diagrams/AS-IS_Container.puml](./diagrams/AS-IS_Container.puml) | Диаграмма C4 Container для текущего состояния (PlantUML) |

| [Diagrams/TO-BE_1Context.puml](./diagrams/TO-BE_1Context.puml) | Диаграмма контекста (System Context) для целевого состояния |
| [Diagrams/TO-BE_2Container.puml](./diagrams/TO-BE_2Container.puml) | Диаграмма C4 Container для целевого состояния (PlantUML) |
| [Diagrams/TO-BE_3Bidding_Component.puml](./diagrams/TO-BE_3Bidding_Component.puml) | Компонентная диаграмма Bidding Service (уровень 3) |
| [Diagrams/TO-BE_3Statistics_Component.puml](./diagrams/TO-BE_3Statistics_Component.puml) | Компонентная диаграмма Statistics Service |
| [Diagrams/TO-BE_3Financial_Component.puml](./diagrams/TO-BE_3Financial_Component.puml) | Компонентная диаграмма Financial Service |
| [Diagrams/TO-BE_4Bidding_Code.puml](./diagrams/TO-BE_4Bidding_Code.puml) | Диаграмма классов (уровень 4) для Bidding Service |

| [adr/ADR-001_Evolution_Strategy.md](./adr/ADR-001_Evolution_Strategy.md) | ADR-001: Стратегия эволюции архитектуры |
| [adr/ADR-002_Bidding_Service_Priority.md](./adr/ADR-002_Bidding_Service_Priority.md) | ADR-002: Выделение сервиса ставок в качестве первого приоритета |
| [adr/ADR-003_Streaming_Platform.md](./adr/ADR-003_Streaming_Platform.md) | ADR-003: Выбор технологии для потоковой обработки событий |



