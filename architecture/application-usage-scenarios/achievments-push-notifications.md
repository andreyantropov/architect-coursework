## Отправка push-уведомления о достижении друга

```mermaid
sequenceDiagram

    participant SG as Социализация и геймификация
    participant E as Event Bus
    participant N as Сервис уведомлений
    participant P as Push Provider
    participant M as Мобильное приложение

    SG->E: AchievementUnlocked
    E->>N: AchievementUnlocked
    N->>P: Отправка push-уведомления
    P-->>M: Уведомление
```

### Описание:
Событие `AchievementUnlocked` подписывается сервисом уведомлений, который рассылает соответствующие push-сообщения друзьям пользователя.