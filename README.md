# Архитектура фитнес-приложения

## Назначение репозитория

Этот репозиторий содержит архитектурные артефакты для фитнес-приложения компании.

## Навигация

1. [Бизнес-цели](./architecture/1-business-goals.md)
2. [Функциональные требования](./architecture/2-functional-requirements/README.md) →
3. [Анализ стейкхолдеров](./architecture/3-stakeholder-analysis.md)
4. [Концептуальная архитектура](./architecture/4-conceptual-architecture.md)
5. [Анализ рисков](./architecture/5-implementation-risks.md)
6. [План реализации](./architecture/6-development-plan.md)
7. [Критические бизнес-сценарии](./architecture/7-critical-business-processes/README.md) →
8. [Атрибуты качества системы](./architecture/8-quality-attributes.md)
9. [Нефункциональные требования](./architecture/9-non-functional-requirements.md)
10. [Архитектурные опции и обоснование выбора](./architecture/10-architectural-options.md)
11. [ADR](./11-adr/README.md) →
12. [Сценарии использования приложения](./architecture/12-application-usage-scenarios/README.md) →
13. [Базовая архитектура](./architecture/13-basic-architecture.md)
14. [Основные представления](./architecture/14-basic-presentations/README.md) →
15. [Анализ рисков и компромиссов архитектуры](./architecture/15-risks-and-trade-offs-analysis.md)
16. [Расчёт стоимости владения системой ](./architecture/16-cost-of-owning.md)

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
