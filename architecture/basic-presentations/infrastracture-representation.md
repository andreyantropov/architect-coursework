## Инфраструктурное представление

```mermaid
graph LR
    subgraph Cloud Provider
        subgraph DMZ
            ext_api[(External API GW)]
            cdn[(CDN - SPA)]
            push_svc[(Push Service)]
        end

        subgraph Internal Network
            int_api[(Internal API GW)]
            k8s[(Kubernetes Cluster)]
            db[(PostgreSQL Cluster)]
            mongo[(MongoDB Atlas)]
            clickhouse[(ClickHouse Cluster)]
            redis[(Redis Cluster)]
            s3[(S3 Bucket)]
        end

        subgraph Data Layer
            kafka[(Kafka Cluster)]
            ai_engine[(AI Engine)]
            mlflow[(MLflow Tracking)]
        end

        subgraph Monitoring
            otel[(OpenTelemetry)]
            grafana[(Grafana)]
            elk[(ELK Stack)]
        end

        ext_api <-->|HTTPS| cdn & push_svc
        ext_api --> int_api
        int_api -->|gRPC| k8s
        k8s -->|SQL| db
        k8s -->|NoSQL| mongo
        k8s -->|Col| clickhouse
        k8s -->|Cache| redis
        k8s -->|Events| kafka
        k8s -->|Data| s3
        k8s -->|Models| mlflow

        kafka -->|Stream| ai_engine
        ai_engine -->|Store| s3
        ai_engine -->|Log| mlflow

        otel -->|Metrics| all_services
        elk -->|Logs| all_services
        grafana -->|Visualization| otel & elk
    end
```

### Особенности реализации:
- **Гибридная архитектура**: микросервисы + serverless для фоновых задач (AI-инференс, аналитика).
- **Multi-cloud стратегия**: Primary DC + Secondary (горячий) + Tertiary (холодный).
- **Infrastructure-as-Code**: Terraform + Helm обеспечивает автоматизированное развертывание.
- **CI/CD**: GitLab CI управляет сборкой, тестированием и деплоем.
- **Контейнеризация**: Docker + Kubernetes для всех сервисов.