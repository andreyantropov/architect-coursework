## Пользователь регистрируется через email

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as External API Gateway
    participant U as Сервис пользователей
    participant DB as PostgreSQL
    participant E as Event Bus

    M->>A: POST /register (email, password)
    A->>U: Перенаправление запроса
    U->>DB: INSERT INTO users
    DB-->>U: Успех
    U->>E: Публикация события UserRegistered
    E-->>U: Ответ "успешная регистрация"
    U-->>M: Ответ "успешная регистрация"
```

### Описание:

Пользователь проходит регистрацию через email и пароль. Данные сохраняются в реляционной БД. Генерируется событие `UserRegistered`, которое может запустить welcome-нотификацию или начисление бонусных баллов.
