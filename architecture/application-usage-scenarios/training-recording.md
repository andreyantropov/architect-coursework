## Запись тренировки (в online-режиме)

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant W as Сервис тренировок
    participant DB as PostgreSQL
    participant E as Event Bus

    M->>A: POST /workouts/start
    A->>W: Запрос на начало тренировки
    W->>DB: INSERT INTO workouts
    DB-->>W: ID тренировки
    W->>E: WorkoutStarted
    E-->>M: Подтверждение начала
```

### Описание:
Пользователь начинает тренировку, фиксируется стартовое событие. Все данные о маршруте и показаниях датчиков будут отправлены отдельно и обработаны позже.