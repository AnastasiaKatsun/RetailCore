@startuml OrderPlatform C4 Context

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()

title C4 Context Diagram - Order Platform (AS‑IS)

' === Акторы ===
Person(customer, "Клиент", "Покупает товары через сайт, мобильное приложение и маркетплейсы")
Person(admin, "Администратор / Поддержка", "Управляет статусами заказов, обрабатывает эскалации и ручные правки")
Person_Ext(marketplaceManager, "Менеджер маркетплейсов", "Контролирует статусы и синхронизацию заказов на внешних площадках")
Person_Ext(logisticsCoordinator, "Координатор логистики", "Отслеживает резервирование слотов и статусы доставки у партнёров")

' === Система ===
System(orderPlatform, "Order Platform", "On‑Premise платформа для приёма, обработки и исполнения заказов, включая интеграции с платёжными и логистическими провайдерами")

' === Внешние системы ===
System_Ext(paymentProviders, "Платёжные провайдеры", "API эквайринга (Tinkoff, Sber, YooKassa и др.) для проведения оплат и получения статусов транзакций")
System_Ext(logisticsPartners, "Логистические партнёры", "API и очереди для резервирования слотов доставки, маршрутизации и передачи статусов")
System_Ext(promoService, "Промо‑сервис", "API расчёта скидок и акций, внешние правила промо‑кампаний")
System_Ext(externalMarketplaces, "Внешние маркетплейсы", "API/Webhooks для синхронизации заказов, остатков и статусов")
System_Ext(legacySources, "Legacy‑источники данных", "ETL‑интеграции (CSV, FTP, старые БД) для загрузки справочников и исторических данных")

' === Связи ===
Rel(customer, orderPlatform, "Размещает и отслеживает заказы", "HTTPS / Web / Mobile / API маркетплейсов")
Rel(admin, orderPlatform, "Вносит ручные правки, управляет статусами и эскалациями", "Admin UI / DB tools")
Rel(marketplaceManager, orderPlatform, "Контролирует синхронизацию и статусы заказов на маркетплейсах", "Admin UI / отчёты")
Rel(logisticsCoordinator, orderPlatform, "Отслеживает доставку и резервирование слотов", "Отчёты / статусы / алерты")

Rel(orderPlatform, paymentProviders, "Проводит платежи и получает статусы транзакций", "HTTPS / REST / Webhooks")
Rel(orderPlatform, logisticsPartners, "Резервирует слоты, передаёт данные доставки и получает статусы", "REST / очереди / пакетные выгрузки")
Rel(orderPlatform, promoService, "Запрашивает расчёт скидок и промо‑правил", "HTTPS / REST")
Rel(orderPlatform, externalMarketplaces, "Синхронизирует заказы, остатки и статусы", "API / Webhooks / периодические выгрузки")
Rel(orderPlatform, legacySources, "Загружает справочники и исторические данные", "Batch / ETL / файловые интеграции")

@enduml