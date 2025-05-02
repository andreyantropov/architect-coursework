## 🎯 Сценарий 1: Пользователь регистрируется через email

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
    E-->>M: Ответ "успешная регистрация"
```

### Описание:
Пользователь проходит регистрацию через email и пароль. Данные сохраняются в реляционной БД. Генерируется событие `UserRegistered`, которое может запустить welcome-нотификацию или начисление бонусных баллов.

---

## 🎯 Сценарий 2: Вход через социальные сети

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as External API Gateway
    participant U as Сервис пользователей
    participant DB as PostgreSQL
    participant S as OAuth Provider
    participant E as Event Bus

    M->>S: Авторизация через Google/Facebook
    S-->>M: Токен
    M->>A: POST /social-login
    A->>U: Перенаправление
    U->>S: Проверка токена
    S-->>U: Информация о пользователе
    U->>DB: Поиск или создание пользователя
    DB-->>U: Результат
    U->>E: Публикация UserLoggedIn
    E-->>M: Токен сессии
```

### Описание:
Поддержка входа через соцсети реализована через OAuth. При успешном входе генерируется событие `UserLoggedIn`.

---

## 🎯 Сценарий 3: Запись тренировки (в online-режиме)

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant W as Сервис тренировок
    participant DB as PostgreSQL
    participant E as Event Bus

    M->>A: POST /workouts/start
    A->>W: Запрос на начало тренировки
    W->>DB: INSERT INTO workouts
    DB-->>W: ID тренировки
    W->>E: WorkoutStarted
    E-->>M: Подтверждение начала
```

### Описание:
Пользователь начинает тренировку, фиксируется стартовое событие. Все данные о маршруте и показаниях датчиков будут отправлены отдельно и обработаны позже.

---

## 🎯 Сценарий 4: Завершение тренировки и отправка данных с GPS

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
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

---

## 🎯 Сценарий 5: Получение персонализированной рекомендации

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant R as Сервис рекомендаций
    participant C as Redis Cache
    participant AI as AI Engine

    M->>A: GET /recommendations
    A->>R: Запрос
    R->>C: Проверка кэша
    alt Есть в кэше
        C-->>R: Данные
    else Нет в кэше
        R->>AI: Запрос инференса
        AI-->>R: Рекомендации
        R->>C: Сохранение в кэш
    end
    R-->>M: Рекомендации
```

### Описание:
Рекомендации по тренировкам могут быть получены из кэша или сгенерированы динамически через AI-движок.

---

## 🎯 Сценарий 6: Отправка push-уведомления о достижении друга

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

---

## 🎯 Сценарий 7: Просмотр лидерборда

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant AN as Сервис аналитики
    participant CH as ClickHouse
    participant C as Redis

    M->>A: GET /leaderboard
    A->>AN: Запрос
    AN->>CH: SELECT * FROM leaderboard
    CH-->>AN: Рейтинг
    AN->>C: Кэширование
    AN-->>M: Лидерборд
```

### Описание:
Лидерборды получаются из колоночной базы данных (ClickHouse) и кэшируются для оптимизации последующих запросов.

---

## 🎯 Сценарий 8: А/В тестирование маркетинговой промоакции

```mermaid
sequenceDiagram
    participant MK as Сервис маркетинговой аналитики
    participant DB as ClickHouse
    participant A as AI Engine
    participant UI as Веб-админка

    MK->>DB: SELECT metrics
    DB-->>MK: Собранные метрики
    MK->>A: Анализ эффективности A/B вариантов
    A-->>UI: Отчет с выводами
```

### Описание:
Маркетологи проводят A/B тестирование промоакций. После сбора данных система анализирует эффективность и предоставляет отчет.

---

## 🎯 Сценарий 9: Синхронизация офлайн-тренировки

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant W as Сервис тренировок
    participant DB as PostgreSQL
    participant E as Event Bus
    participant L as Local SQLite

    M->>L: Сохранение тренировки локально
    L-->>M: Подтверждение
    alt Подключение восстановлено
        M->>A: POST /sync-workout
        A->>W: Передача данных
        W->>DB: INSERT INTO workouts
        W->>E: WorkoutCompleted
    end
```

### Описание:
Пользователь тренируется без интернета. Данные сохраняются локально и отправляются при восстановлении связи.

---

## 🎯 Сценарий 10: Получение прогноза прогресса

```mermaid
sequenceDiagram
    participant M as Мобильное приложение
    participant A as Internal API Gateway
    participant AI as AI Engine
    participant F as Feature Store
    participant E as Event Bus

    M->>A: GET /forecast
    A->>AI: Запрос предиктивной аналитики
    AI->>F: Загрузка исторических данных
    F-->>AI: Фичи
    AI->>AI: Прогнозирование
    AI-->>M: Прогноз прогресса
```

### Описание:
AI Engine использует данные о предыдущих тренировках для прогнозирования будущего прогресса пользователя.
