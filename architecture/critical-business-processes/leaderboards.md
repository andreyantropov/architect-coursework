## Лидерборды не обновляются
```mermaid
sequenceDiagram
    participant Сервис_Тренировок
    participant Event_Bus
    participant Сервис_Аналитики
    participant Пользователь
    
    Сервис_Тренировок->>Event_Bus: Publish workout_completed
    Event_Bus->>Сервис_Аналитики: Deliver event
    Сервис_Аналитики-->>Event_Bus: HTTP 500 (Internal Error)
    Пользователь->>Пользователь: GET /leaderboard → stale data
    note right of Сервис_Аналитики: Dead letter queue activated
```