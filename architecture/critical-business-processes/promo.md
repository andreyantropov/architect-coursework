## Обработка промоакций
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