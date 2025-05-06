## Безопасность

```mermaid
graph TD
    A[Мобильное приложение] -->|TLS 1.3| B[External API Gateway]
    B -->|mTLS/JWT| D[Internal API Gateway]
    D -->|RBAC| E[Сервис пользователей]
    D -->|JWT| F[Сервис тренировок]
    D -->|ACL| G[Сервис аналитики]

    H[SIEM] -->|Audit Logs| I[(ELK Stack)]
    J[Firewall] -->|DDoS Protection| B
    K[Consent Management] -->|GDPR| L[Database Encryption]
    M[Feature Store] -->|Anonymized Data| N[AI Training]
    O[Session Store] -->|Encrypted Redis| P[Token Revocation List]
    Q[HashiCorp Vault] -->|Secrets| R[All Services]
```

### Особенности реализации:

- **DMZ** содержит только внешние точки входа: API Gateway, CDN, Push Service.
- Все внутренние вызовы осуществляются через **mutual TLS (mTLS)** и **JWT**.
- **Шифрование**: TLS 1.3 для передачи, AES-256 для хранения PII.
- **GDPR/CCPA Compliance**: политики удаления данных, consent management.
- **SIEM-система** коррелирует события из DMZ и внутренней сети для обнаружения аномалий.
- **AI-модели** обучаются на **анонимизированных данных** с применением дифференциальной приватности.
- **Сессии** и токены хранятся в зашифрованном Redis.
- **HashiCorp Vault** управляет секретами и сертификатами.
