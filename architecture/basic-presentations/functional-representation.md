## Функциональное представление

### Структура модулей

```mermaid
graph TD
    A[Пользователи и безопасность] -->|auth| B[Тренировки]
    A -->|profile| C[Социализация]
    A -->|roles| D[Рекомендации]
    A -->|consent| E[Безопасность]

    B -->|metrics| F[Аналитика тренировок]
    B -->|events| G[AI Engine]

    C -->|activity feed| F
    C -->|achievements| H[Уведомления]

    D -->|personalization| B
    D -->|promotions| I[Маркетинговая аналитика]

    G -->|models| D
    G -->|predictive| J[Прогнозирование прогресса]

    H -->|push/email| K[Push Service]
    H -->|in-app| L[Redis Cache]

    I -->|A/B testing| M[CRM интеграция]
```

### Особенности реализации:

- **Пользователи и безопасность** — центральный микросервис, от которого зависят почти все остальные.
- **CQRS** используется в **тренировках** и **аналитике**: запись — через PostgreSQL, чтение — через MongoDB / ClickHouse.
- **Event Sourcing** применяется для критичных событий: `UserRegistered`, `WorkoutStarted`, `WorkoutCompleted`.
- **AI Engine** подписывается на события и генерирует рекомендации/прогнозы, которые публикуются обратно в систему.
