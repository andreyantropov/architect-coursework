## Регистрация нового пользователя
```mermaid
sequenceDiagram
    participant Пользователь
    participant API_Gateway
    participant Сервис_Пользователей
    participant SMTP_Service
    
    Пользователь->>API_Gateway: POST /register
    API_Gateway->>Сервис_Пользователей: Forward request
    Сервис_Пользователей->>SMTP_Service: Send verification email
    SMTP_Service-->>Сервис_Пользователей: SMTP 550 (Mailbox unavailable)
    Сервис_Пользователей-->>API_Gateway: HTTP 424 (Failed Dependency)
    API_Gateway-->>Пользователь: "Ошибка: Не удалось отправить письмо подтверждения"
    note left of SMTP_Service: Fallback: SMS verification
```