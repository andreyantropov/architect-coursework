## Инфраструктурное представление

```mermaid
graph LR
    subgraph Cloud Provider
        subgraph DMZ
            ext_api[(External API GW)]
            cdn[(CDN - SPA)]
            push_svc[(Push Service)]
            analytics_gw[(Analytics API Gateway)]
        end

        subgraph Internal Network
            int_api[(Internal API GW)]
            k8s_workers[(Kubernetes Cluster)]
            db_postgres[(PostgreSQL Cluster)]
            mongo[(MongoDB Atlas)]
            clickhouse[(ClickHouse Cluster)]
            redis[(Redis Cluster)]
            s3[(S3 Bucket)]
            vault[(HashiCorp Vault)]
        end

        subgraph Data Layer
            kafka[(Kafka Cluster)]
            nats_js[(NATS JetStream)]
            ai_engine[(AI Engine)]
            mlflow[(MLflow Tracking)]
            delta_lake[(Delta Lake)]
        end

        subgraph Monitoring
            otel[(OpenTelemetry)]
            grafana[(Grafana)]
            elk[(ELK Stack)]
        end

        ext_api <-->|HTTPS| cdn & push_svc & analytics_gw
        ext_api --> int_api
        int_api -->|gRPC| k8s_workers
        k8s_workers -->|SQL| db_postgres
        k8s_workers -->|NoSQL| mongo
        k8s_workers -->|Col| clickhouse
        k8s_workers -->|Cache| redis
        k8s_workers -->|Events| kafka
        k8s_workers -->|Messages| nats_js
        k8s_workers -->|Data| s3
        k8s_workers -->|Models| mlflow
        k8s_workers -->|Secrets| vault

        kafka -->|Stream| ai_engine
        nats_js -->|Realtime| H[Уведомления]
        ai_engine -->|Store| s3
        ai_engine -->|Log| mlflow
        delta_lake -->|Features| ai_engine

        otel -->|Metrics| all_services
        elk -->|Logs| all_services
        grafana -->|Visualization| otel & elk
    end
```

### Особенности реализации:

- **Гибридная архитектура**: микросервисы + serverless (Lambda) для фоновых задач (AI-инференс, аналитика).
- **Multi-cloud стратегия**: Primary DC + Secondary (горячий) + Tertiary (холодный).
- **Infrastructure-as-Code**: Terraform + Helm обеспечивает автоматизированное развертывание.
- **CI/CD**: GitLab CI управляет сборкой, тестированием и деплоем.
- **Контейнеризация**: Docker + Kubernetes для всех сервисов.
- **Шифрование**: TLS 1.3 для передачи, AES-256 для хранения PII.
