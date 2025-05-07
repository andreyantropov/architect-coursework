## Отправка push-уведомления о достижении друга

```mermaid
sequenceDiagram
    participant W as Сервис тренировок
    participant E as Event Bus
    participant A as Сервис аналитики тренировок
    participant SG as Сервис социализации
    participant N as Сервис уведомлений
    participant P as Push Provider
    participant M as Мобильное приложение

    W->>E: WorkoutCompleted (user_id, workout_id, stats)
    E-->>A: Событие WorkoutCompleted
    A->>A: Обновление метрик пользователя
    A->>E: UserMetricsUpdated (distance_total, days_streak, avg_speed)

    E-->>SG: UserMetricsUpdated
    SG->>SG: Проверяет условия достижения
    alt Достижение разблокировано
        SG->>E: AchievementUnlocked (user_id, achievement_id)
    end

    E-->>N: AchievementUnlocked
    N->>P: Отправка push-уведомления друзьям
    P-->>M: Уведомление "Друг достиг нового уровня!"
```

### Описание:

Событие `AchievementUnlocked` подписывается сервисом уведомлений, который рассылает соответствующие push-сообщения друзьям пользователя.
