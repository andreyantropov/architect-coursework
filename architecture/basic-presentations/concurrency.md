## Многозадачность / Concurrency

```mermaid
sequenceDiagram
    participant U as User
    participant API as API Gateway
    participant WS as Workout Service
    participant AS as Analytics Service
    participant AE as AI Engine
    participant EB as Event Bus
    participant DB as PostgreSQL
    participant CH as ClickHouse

    U->>API: Начать тренировку
    API->>WS: Запрос на создание
    WS->>DB: INSERT INTO workouts
    DB-->>WS: ID созданной тренировки
    WS->>EB: Публикация события WorkoutStarted
    EB->>AS: Обновление лидербордов
    EB->>AE: Запрос на пересчет рекомендаций
    AE->>AE: Обучение модели
    AE->>EB: Рекомендации готовы
    EB->>WS: Получение новых рекомендаций
    WS-->>U: Подтверждение начала тренировки
```

### Особенности реализации:
- Используется **event-driven подход** для асинхронного обновления состояния других сервисов.
- **Kafka** гарантирует доставку событий даже при высокой нагрузке.
- **Redis** используется для кэширования частых операций (рейтинги, уведомления).
- **Async processing** для AI-инференса и аналитики позволяет избежать блокировки основного потока.