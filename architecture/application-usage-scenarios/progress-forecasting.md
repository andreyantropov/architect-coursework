## Получение прогноза прогресса

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant AI as AI Engine
    participant F as Feature Store
    participant E as Event Bus

    M->>A: GET /forecast
    A->>AI: Запрос предиктивной аналитики
    AI->>F: Загрузка исторических данных
    F-->>AI: Фичи
    AI->>AI: Прогнозирование
    AI-->>M: Прогноз прогресса
```

### Описание:
AI Engine использует данные о предыдущих тренировках для прогнозирования будущего прогресса пользователя.