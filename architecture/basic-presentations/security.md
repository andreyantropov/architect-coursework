## Безопасность

```mermaid
graph TD
    A[Мобильное приложение] -->|TLS 1.3| B[External API Gateway]
    B -->|mTLS| C[Internal API Gateway]
    C -->|RBAC| D[Сервис пользователей]
    C -->|JWT| E[Сервис тренировок]
    C -->|ACL| F[Сервис аналитики]

    G[SIEM] -->|Audit Logs| H[(ELK Stack)]
    I[Firewall] -->|DDoS Protection| B
    J[Consent Management] -->|GDPR| K[Database Encryption]
    L[Feature Store] -->|Anonymized Data| M[AI Training]
    N[Session Store] -->|Encrypted Redis| O[Token Revocation List]
```

### Особенности реализации:
- **DMZ** содержит только внешние точки входа: API Gateway, CDN, Push Service.
- Все внутренние вызоры осуществляются через **mutual TLS (mTLS)** и **JWT**.
- **Шифрование**: TLS 1.3 для передачи, AES-256 для хранения PII.
- **GDPR/CCPA Compliance**: политики удаления данных, consent management.
- **SIEM-система** коррелирует события из DMZ и внутренней сети для обнаружения аномалий.
- **AI-модели** обучаются на **анонимизированных данных** с применением дифференциальной приватности.
- **Сессии** и токены хранятся в зашифрованном Redis.
