## Пользователь не может войти в систему
```mermaid
sequenceDiagram
    participant Пользователь
    participant API_Gateway
    participant Сервис_Авторизации
    
    Пользователь->>API_Gateway: POST /auth/login
    API_Gateway->>Сервис_Авторизации: Forward request
    Сервис_Авторизации-->>API_Gateway: HTTP 401 (Invalid credentials)
    API_Gateway-->>Пользователь: "Ошибка: Неверный логин или пароль"
    note right of Сервис_Авторизации: Логирование попытки<br>Блокировка при 5 ошибках
```