# Критические бизнес-сценарии

## 1. Пользователь не может войти в систему
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

## 2. Пользователь не может начать тренировку
```mermaid
sequenceDiagram
    participant Пользователь
    participant API_Gateway
    participant Сервис_Тренировок
    
    Пользователь->>API_Gateway: POST /workouts/start
    API_Gateway->>Сервис_Тренировок: Forward request
    Сервис_Тренировок-->>API_Gateway: HTTP 503 (Service Unavailable)
    API_Gateway-->>Пользователь: "Ошибка: Сервис тренировок временно недоступен"
    note left of Сервис_Тренировок: Автоматическое оповещение SRE
```

## 3. Лидерборды не обновляются
```mermaid
sequenceDiagram
    participant Сервис_Тренировок
    participant Event_Bus
    participant Сервис_Аналитики
    participant Пользователь
    
    Сервис_Тренировок->>Event_Bus: Publish workout_completed
    Event_Bus->>Сервис_Аналитики: Deliver event
    Сервис_Аналитики-->>Event_Bus: HTTP 500 (Internal Error)
    Пользователь->>Пользователь: GET /leaderboard → stale data
    note right of Сервис_Аналитики: Dead letter queue activated
```

## 4. Регистрация нового пользователя
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

## 5. Интеграция с фитнес-устройством 
```mermaid
sequenceDiagram
    participant Пользователь
    participant API_Gateway
    participant Сервис_Интеграции
    participant Device_API
    
    Пользователь->>API_Gateway: POST /devices/connect
    API_Gateway->>Сервис_Интеграции: Forward request
    Сервис_Интеграции->>Device_API: OAuth2 token request
    Device_API--x Сервис_Интеграции: Connection timeout
    Сервис_Интеграции->>Сервис_Интеграции: 3 retry attempts
    Сервис_Интеграции-->>API_Gateway: HTTP 502 (Bad Gateway)
    API_Gateway-->>Пользователь: "Ошибка: Сервис устройства не отвежает"
    note right of Device_API: Circuit breaker pattern
```

## 6. Создание групповой тренировки
```mermaid
sequenceDiagram
    participant Тренер
    participant API_Gateway
    participant Сервис_Тренировок
    participant Сервис_Уведомлений
    
    Тренер->>API_Gateway: POST /group-workouts
    API_Gateway->>Сервис_Тренировок: Forward request
    Сервис_Тренировок->>Сервис_Уведомлений: Send invitations
    Сервис_Уведомлений-->>Сервис_Тренировок: HTTP 429 (Too Many Requests)
    Сервис_Тренировок-->>API_Gateway: HTTP 424 (Failed Dependency)
    API_Gateway-->>Тренер: "Ошибка: Лимит уведомлений превышен"
    note left of Сервис_Уведомлений: Exponential backoff
```

## 7. Обработка промоакций
```mermaid
sequenceDiagram
    participant Админ
    participant Сервис_Промо
    participant Сервис_Рекомендаций
    participant Сервис_Пользователей
    
    Админ->>Сервис_Промо: POST /promotions
    Сервис_Промо->>Сервис_Рекомендаций: Request segmentation
    Сервис_Рекомендаций->>Сервис_Пользователей: GET /users/segments
    Сервис_Пользователей-->>Сервис_Рекомендаций: HTTP 504 (Gateway Timeout)
    Сервис_Рекомендаций-->>Сервис_Промо: HTTP 424 (Failed Dependency)
    Сервис_Промо->>Админ: "Ошибка: Таймаут сегментации пользователей"
    note right of Сервис_Пользователей: Cache fallback
```

## 8. Восстановление пароля
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