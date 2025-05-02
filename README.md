# Архитектура фитнес-приложения

## Назначение репозитория

Этот репозиторий содержит архитектурные артефакты для фитнес-приложения компании.

## Навигация

1. [Бизнес-цели](./architecture/business-goals.md)
2. [Функциональные требования](./architecture/functional-requirements/README.md) →
3. [Анализ стейкхолдеров](./architecture/stakeholder-analysis.md)
4. [Концептуальная архитектура](.//conceptual-architecture.md)
5. [Анализ рисков](./architecture/implementation-risks.md)
6. [План реализации](./architecture/development-plan.md)
7. [Критические бизнес-сценарии](./architecture/critical-business-processes/README.md) →
8. [Атрибуты качества системы](./architecture/quality-attributes.md)
9. [Нефункциональные требования](.//non-functional-requirements.md)
10. [Архитектурные опции и обоснование выбора](./architecture/architectural-options.md)
11. [ADR](./adr/README.md) →
12. [Сценарии использования приложения](./architecture/application-usage-scenarios/README.md) →
13. [Базовая архитектура](./architecture/basic-architecture.md)
14. [Основные представления](./architecture/basic-presentations/README.md) →

## Краткое описание решения

### Ключевые особенности

- **Социальная интеграция**: Группы по интересам, совместные тренировки
- **Геймификация**: Лидерборды, достижения, соревнования
- **Персонализация**: Рекомендации на основе инвентаря
- **Глобальная доступность**: Поддержка 10+ языков
- **Безопасность**: Соответствие GDPR/CCPA

### Технологический стек

- **Бэкенд**: Go/Python (микросервисы)
- **Фронтенд**: React Native/Next.js
- **Инфраструктура**: Kubernetes + Istio
- **Аналитика**: ClickHouse + Apache Kafka
- **AI/ML**: PyTorch + S3
