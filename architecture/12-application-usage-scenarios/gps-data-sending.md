## Завершение тренировки и отправка данных с GPS

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as External API Gateway
    participant W as Сервис тренировок
    participant DB as PostgreSQL
    participant E as Event Bus
    participant AI as AI Engine

    M->>A: PUT /workouts/{id}
    A->>W: Обновление данных тренировки
    W->>DB: UPDATE workouts SET ...
    W->>E: WorkoutCompleted
    E->>AI: Обучение модели рекомендаций
    DB-->>W: Успех
    E-->>M: Подтверждение завершения
```

### Описание:
После окончания тренировки отправляются полные данные (маршрут, показания датчиков). Это событие триггерит машинное обучение для дальнейших рекомендаций.