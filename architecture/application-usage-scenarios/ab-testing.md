## А/В тестирование маркетинговой промоакции

```mermaid
sequenceDiagram
    participant UI as Веб-админка (маркетинг)
    participant GW as Internal API Gateway
    participant MK as Маркетинговый сервис
    participant DB as ClickHouse
    participant AI as AI Engine
    participant REPORT as BI-панель (Redash)

    UI->>GW: GET /marketing/campaigns?test_id=AB123
    GW->>MK: Перенаправление запроса
    MK->>DB: SELECT * FROM marketing_metrics WHERE campaign_id = 'AB123'
    DB-->>MK: Данные по кампании
    MK->>AI: POST /analyze-ab-test (campaign_id, metrics)
    AI->>DB: Запрос дополнительных данных
    DB-->>AI: Дополнительные метрики
    AI->>AI: Анализ эффективности A/B вариантов
    AI-->>MK: Результаты анализа
    MK-->>GW: Ответ с данными
    GW-->>UI: JSON-ответ с метриками и выводами
    UI->>REPORT: Визуализация отчёта
```

### Описание:

Маркетологи проводят A/B тестирование промоакций. После сбора данных система анализирует эффективность и предоставляет отчет.
