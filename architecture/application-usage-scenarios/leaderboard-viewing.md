## Просмотр лидерборда

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as External API Gateway
    participant TA as Аналитика тренировок
    participant C as Redis
    participant CH as ClickHouse

    M->>A: GET /leaderboard
    A->>TA: Перенаправление запроса
    TA->>C: Проверка кэша (leaderboard_{sport}_{region})
    alt Кэш найден
        C-->>TA: Возврат данных из кэша
    else Кэш отсутствует или устарел
        TA->>CH: SELECT * FROM leaderboard WHERE sport = ? AND region = ?
        CH-->>TA: Рейтинг
        TA->>C: Сохранение в кэш (с TTL)
    end
    TA-->>M: Лидерборд
```

### Описание:

Лидерборды получаются из колоночной базы данных и кэшируются для оптимизации последующих запросов.
