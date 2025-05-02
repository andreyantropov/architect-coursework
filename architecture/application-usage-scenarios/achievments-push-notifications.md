## Отправка push-уведомления о достижении друга

```mermaid
sequenceDiagram
    participant E as Event Bus
    participant AI as AI Engine
    participant N as Сервис уведомлений
    participant P as Push Provider
    participant M as Мобильное приложение

    E->>N: AchievementUnlocked
    N->>P: Отправка push-уведомления
    P-->>M: Уведомление
```

### Описание:
Событие `AchievementUnlocked` подписывается сервисом уведомлений, который рассылает соответствующие push-сообщения друзьям пользователя.