# 🚀 ELT Data Platform (Production-Ready)

This project demonstrates the design of a **modern ELT data pipeline** that is scalable, secure, and production-ready.  
It covers the complete lifecycle of data: from ingestion and storage, to transformation, quality validation, lineage tracking, observability, and serving to BI tools.  

---

## 🎯 Objective
Build a **reliable and auditable ELT platform** to collect, store, transform, validate, monitor, and serve data for analytics and business operations.

---

## 🏗 High-Level Architecture

1. **Sources** → Databases, APIs, CSVs, Event Streams (Kafka/Kinesis).  
2. **Ingestion** → Airbyte / Fivetran (CDC-enabled) for Extract + Load.  
3. **Landing (Raw Storage)** → Object Storage (S3 / ADLS / GCS).  
4. **Warehouse (ELT Target)** → Snowflake / BigQuery / Redshift.  
5. **Transformation (dbt)** → Staging → Intermediate → Marts.  
6. **Data Quality (Great Expectations + dbt tests)** → Validation of data contracts and business rules.  
7. **Lineage & Catalog (OpenLineage + Marquez / DataHub)** → Full data lineage and metadata management.  
8. **Storage Catalog / Docs (dbt docs)** → Auto-generated documentation and model lineage.  
9. **Serving & BI** → Looker / Power BI / Superset.  
10. **Observability** → CloudWatch / Prometheus + Grafana (logs, metrics, alerts).  
11. **Orchestration** → Airflow / Dagster to manage DAGs, retries, SLAs.

---

## 🔐 Security

- **IAM / RBAC**: Principle of Least Privilege, Service Accounts (no hardcoded credentials).  
- **Encryption**:  
  - At Rest: S3 KMS, Snowflake TDE.  
  - In Transit: TLS everywhere.  
- **Data Governance**:  
  - Row-Level Security & Masking for PII.  
  - Audit logs in Warehouse.  
  - Lineage registered in DataHub/Marquez.  

---

## ✅ Data Quality

- **dbt tests** → Unique, Not Null, Relationships.  
- **Great Expectations** → Advanced checks: distributions, regex, ranges.  
- **Data Contracts** → Enforced schema agreements with source teams.  
- **Fail Fast** → Critical test failures block downstream publish.  
- **Data Docs** → Auto-generated reports for transparency.

---

## 📊 Observability & Logging

- **Pipeline Monitoring**:  
  - Airflow task metrics (success/failure, SLA, duration).  
  - SLA alerts if DAG exceeds defined thresholds.  
- **Data Monitoring**:  
  - Freshness of key tables.  
  - Volume anomalies.  
  - Schema drift detection.  
- **Logs**:  
  - Airflow → ELK / CloudWatch.  
  - Ingestion / dbt logs persisted and centralized.  

---

## ⏱ Orchestration

- **Airflow / Dagster** to orchestrate the entire pipeline.  
- **Dependency Management**: Ingestion → Landing → Warehouse → dbt → GE → Publish.  
- **Retries**: 3 retries with exponential backoff.  
- **SLA**: Critical DAGs complete by 6AM.  
- **Triggers**: Event-based scheduling supported.  
- **OpenLineage Integration**: Each operator emits lineage metadata.

---

## 🗂 Documentation & Catalog

- **dbt docs** → Auto-generated model documentation + internal lineage graph.  
- **DataHub / Amundsen** → Metadata, catalog, ownership tracking.  
- **Lineage Graph** → From raw source to BI dashboards.  

---

## 💾 Backup & Disaster Recovery

- **Storage** → S3/ADLS versioning + lifecycle policies.  
- **Warehouse** → Snowflake Time Travel, BigQuery snapshots.  
- **Airflow** → RDS snapshot & HA deployment.  
- **Recovery Testing** → Regular DR drills.  

---

## 🚨 Incident Response

- **Alert Channels** → Slack, PagerDuty, Email.  
- **Alert Types**:  
  - Pipeline failure (Airflow DAG/task).  
  - Data quality test failure.  
  - Freshness/volume anomalies.  
  - Unauthorized access (security).  
- **Runbooks** → Step-by-step guides for issue resolution.  

---

## 📈 SLAs & SLOs

- **Pipeline Success Rate**: ≥ 99%  
- **Data Freshness**: ≤ 15 minutes delay  
- **Data Quality**: ≥ 99.5% of critical tests passing  
- **MTTD (Mean Time to Detect)**: < 5 minutes  
- **MTTR (Mean Time to Recover)**: < 30 minutes  

---

## 🛠 Development Workflow (CI/CD)

- **GitOps** → All changes tracked in Git.  
- **CI**:  
  - Lint + compile dbt models.  
  - Run unit tests (pytest for utilities).  
  - `dbt build --select state:modified`.  
- **CD**:  
  - Deploy Airflow DAGs + dbt models to staging → prod.  
- **Feature Branching**: All changes via PRs.  
- **Rollback**: Blue/green deployments or version pinning.  

---

## 🔢 ELT Steps (End-to-End Flow)

1. **Sources** → Raw data from DBs/APIs/CSV/Events.  
2. **Ingestion** (Airbyte/Fivetran with CDC).  
3. **Landing** → Raw storage in S3/ADLS/GCS.  
4. **Warehouse** → Load raw into Snowflake/BigQuery/Redshift.  
5. **Transformations** → dbt models build staging → marts.  
6. **Data Quality (GE + dbt tests)** → Validate data quality, fail = stop + alert.  
7. **Lineage & Catalog (OpenLineage)** → Track lineage + metadata.  
8. **Docs (dbt docs)** → Auto-generated documentation.  
9. **Serving & BI** → Clean data to Looker/Power BI/Superset.  
10. **Observability** → Metrics, logs, alerts in CloudWatch/Grafana.  
11. **Orchestration** → Airflow/Dagster manages full cycle.  

📌 Order of execution:  
`1 → 2 → 3 → 4 → 5 → 6 (GE) → 7 (OpenLineage) → 8/9/10 → managed by 11 (Airflow)`  


