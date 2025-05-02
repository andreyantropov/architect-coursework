## Восстановление пароля
```mermaid
sequenceDiagram
    participant Пользователь
    participant API_Gateway
    participant Сервис_Аутентификации
    
    Пользователь->>API_Gateway: POST /password-reset
    API_Gateway->>Сервис_Аутентификации: Forward request
    Сервис_Аутентификации->>Сервис_Аутентификации: Generate token (failed)
    Сервис_Аутентификации-->>API_Gateway: HTTP 500 (Key generation error)
    API_Gateway-->>Пользователь: "Ошибка: Внутренняя ошибка сервера"
    note left of Сервис_Аутентификации: HSM failure alert
```