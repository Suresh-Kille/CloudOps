# CloudOps: Intelligent Cloud Infrastructure Monitoring and Automated DevOps Management System

[![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue.svg)](https://github.com)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI%200.115-009688.svg)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/Frontend-React%2018%20%7C%20Tailwind-61DAFB.svg)](https://react.dev)
[![Scikit-Learn](https://img.shields.io/badge/ML-Isolation%20Forest%20(F1%3A%2090.4%25)-F7931E.svg)](https://scikit-learn.org)
[![Prometheus](https://img.shields.io/badge/Monitoring-Prometheus%20%2B%20Grafana-E6522C.svg)](https://prometheus.io)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An intelligent, full-stack Cloud Infrastructure Monitoring, Anomaly Detection, and Controlled Self-Healing platform developed as a B.Tech final-year CSE (Data Science) academic capstone project based on a six-week summer internship in Cloud Infrastructure & DevOps Engineering.

---

## 🌟 Key Highlights

- **100% Zero-Cloud Cost:** Runs completely locally on `localhost` without requiring AWS, GCP, or paid SaaS subscriptions.
- **Real-Time Telemetry Harvesting:** Continuous ingestion of host CPU, RAM, Disk, Network I/O, and container health.
- **Scikit-Learn Isolation Forest Engine:** Evaluated on a 1,200-sample benchmark dataset achieving **90.40% F1-score**, **99.12% Recall**, and **0.9890 ROC-AUC**.
- **Controlled Self-Healing:** Policy-governed service remediation with strict allowlist enforcement, mandatory 120s cooldowns, and 3-retry limits.
- **Enterprise Dark-Themed UI:** Single-pane React 18 executive dashboard featuring 11 views, live polling, and downloadable CSV audits.
- **Production DevOps Automation:** Docker Compose 6-service orchestration and GitHub Actions CI/CD pipeline with 13 automated test suites.

---

## 🏛️ System Architecture

```
+-------------------------------------------------------------------------------+
|                             CLOUDOPS ARCHITECTURE                             |
+-------------------------------------------------------------------------------+

[ Client Browser: React 18 + Tailwind CSS Dashboard (Port 3000 / 8000) ]
                                |
                                | REST APIs & JSON Web Tokens
                                v
+-------------------------------------------------------------------------------+
|                    FASTAPI APPLICATION SERVER (Port 8000)                    |
|                                                                               |
|  +-------------------+  +-------------------+  +---------------------------+  |
|  | Auth & RBAC       |  | Metrics Collector |  | ML Anomaly Engine         |  |
|  | JWT + Bcrypt      |  | psutil + Telemetry|  | Isolation Forest (.joblib)|  |
|  +-------------------+  +-------------------+  +---------------------------+  |
|                                                                               |
|  +-------------------+  +-------------------+  +---------------------------+  |
|  | Alerts Manager    |  | Incident Tracker  |  | Controlled Self-Healing   |  |
|  | Severity Rules    |  | Post-Mortems      |  | Allowlist + Cooldown Guard|  |
|  +-------------------+  +-------------------+  +---------------------------+  |
+-------------------------------------------------------------------------------+
         |                              |                               |
         | Prometheus Exporter          | SQLAlchemy ORM                | Docker Engine API
         v                              v                               v
[ Prometheus (9090) ]          [ PostgreSQL / SQLite ]         [ Monitored Containers ]
         |                              |                       - auth-service
         v                              v                       - api-gateway
[ Grafana Dashboard (3001) ]   [ Audit & Telemetry DB ]         - metrics-collector
                                                                - frontend-portal
                                                                - worker-service
```

---

## 💻 Technology Stack

| Domain | Technologies |
| :--- | :--- |
| **Frontend** | React 18, Tailwind CSS (Dark Mode), Recharts Data Visualization, Axios |
| **Backend** | Python 3.12, FastAPI (Asynchronous ASGI), Pydantic v2, SQLAlchemy 2.0 ORM |
| **Database** | PostgreSQL 15 (with zero-configuration SQLite 3 fallback for instant local running) |
| **Machine Learning** | Scikit-learn (Isolation Forest), Pandas, NumPy, StandardScaler |
| **DevOps & Containers** | Docker, Docker Compose, Multi-Stage Dockerfiles, GitHub Actions CI/CD |
| **Monitoring** | Prometheus Scraper (`/metrics`), Grafana 10.3 Dashboard, Google cAdvisor |

---

## 📁 Project Structure

```
CloudOps/
|-- frontend/
|   |-- src/
|   |   |-- App.jsx                 # Full 11-page React 18 Dashboard application
|   |-- index.html                  # HTML5 Mount with Tailwind CDN & Babel JSX
|   |-- package.json                # Frontend dependencies & build scripts
|   |-- vite.config.js              # Vite dev server & API proxy config
|   |-- tailwind.config.js          # Dark theme color configuration
|   |-- Dockerfile                  # Multi-stage production Nginx container build
|
|-- backend/
|   |-- app/
|   |   |-- api/
|   |   |   |-- endpoints.py        # REST API endpoints (Auth, Metrics, Containers, Alerts, Healing)
|   |   |-- core/
|   |   |   |-- config.py           # Environment & security settings
|   |   |   |-- database.py         # SQLAlchemy engine & session manager
|   |   |   |-- security.py         # JWT tokens & Bcrypt password hashing
|   |   |-- models/
|   |   |   |-- entities.py         # 8 Relational database tables
|   |   |-- schemas/
|   |   |   |-- schemas.py          # Pydantic v2 validation models
|   |   |-- monitoring/
|   |   |   |-- collector.py        # Non-blocking psutil probe & Prometheus exporter
|   |   |-- ml/
|   |   |   |-- inference.py        # Real-time anomaly scoring engine
|   |   |-- alerts/
|   |   |   |-- alert_engine.py     # Severity rules & incident escalation
|   |   |-- recovery/
|   |   |   |-- healer.py           # Controlled self-healing with allowlist & cooldowns
|   |   |-- main.py                 # FastAPI application mount & static server
|   |-- tests/
|   |   |-- test_api.py             # 13 automated pytest test suites (100% pass rate)
|   |-- requirements.txt            # Python dependencies
|   |-- Dockerfile                  # Multi-stage Python 3.12 slim container
|
|-- ml-engine/
|   |-- datasets/
|   |   |-- generate_dataset.py     # Workload generator with ground-truth failure injections
|   |   |-- infrastructure_metrics_dataset.csv
|   |-- preprocessing/
|   |   |-- preprocessor.py         # Rolling stats, delta features & StandardScaler
|   |-- training/
|   |   |-- train.py                # Isolation Forest training script
|   |-- evaluation/
|   |   |-- evaluate.py             # Benchmark evaluation script (F1, Recall, ROC-AUC)
|   |   |-- evaluation_report.json  # Stored evaluation metrics
|   |-- models/
|   |   |-- isolation_forest.joblib # Serialized Isolation Forest model
|   |   |-- scaler.joblib           # Serialized feature preprocessor
|   |   |-- model_metadata.json     # Hyperparameters & feature columns metadata
|
|-- monitoring/
|   |-- prometheus.yml              # Prometheus scraper configuration
|   |-- grafana/
|       |-- dashboards/
|       |   |-- cloudops_overview.json # Pre-built Grafana executive dashboard
|       |-- provisioning/
|           |-- datasources/datasource.yml
|           |-- dashboards/dashboards.yml
|
|-- database/
|   |-- init.sql                    # PostgreSQL schema initialization & seed data
|
|-- deployment/
|   |-- docker-compose.yml          # 6-service orchestration
|   |-- scripts/
|       |-- start_local.bat         # 1-click Windows local launcher
|       |-- start_local.sh          # Linux/macOS launcher
|       |-- docker_start.bat        # 1-click Docker Compose launcher
|       |-- docker_start.sh
|
|-- docs/
|   |-- PROJECT_REPORT.md           # 24-section comprehensive academic report
|   |-- CloudOps_Presentation.pptx  # 12-slide executive PowerPoint presentation
|   |-- PRESENTATION_SLIDES.md      # Slide outlines & defense speaker notes
|   |-- DEMONSTRATION_GUIDE.md      # Step-by-step evaluator demonstration guide
|   |-- VIVA_PREPARATION.md         # 35 comprehensive viva questions & answers
|
|-- .github/workflows/
|   |-- ci.yml                      # GitHub Actions CI/CD automated workflow
|
|-- .env.example
|-- .gitignore
|-- README.md
```

---

## 🚀 Quickstart Guide

### Option 1: Turnkey Local Launch (Recommended for Evaluation)

No Docker or PostgreSQL setup required! Starts with default SQLite database and pre-trained ML models.

```bash
# Windows
deployment\scripts\start_local.bat

# Or manually:
python -m uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --reload
```

Open your browser and navigate to:
- **CloudOps Dashboard:** [http://localhost:8000](http://localhost:8000)
- **Interactive Swagger API Docs:** [http://localhost:8000/docs](http://localhost:8000/docs)
- **Prometheus Telemetry Scrape:** [http://localhost:8000/metrics](http://localhost:8000/metrics)

**Default Demo Credentials:**
- **Username:** `admin`
- **Password:** `admin123`

---

### Option 2: Full Docker Compose Deployment

Spins up all 6 microservices (Postgres, Backend, Frontend, Prometheus, Grafana, cAdvisor):

```bash
# Windows
deployment\scripts\docker_start.bat

# Linux / macOS
docker compose -f deployment/docker-compose.yml up --build -d
```

**Service URLs:**
- **Frontend Dashboard:** [http://localhost:3000](http://localhost:3000)
- **Backend API:** [http://localhost:8000](http://localhost:8000)
- **Prometheus UI:** [http://localhost:9090](http://localhost:9090)
- **Grafana Dashboard:** [http://localhost:3001](http://localhost:3001) *(admin / admin)*
- **cAdvisor Metrics:** [http://localhost:8080](http://localhost:8080)

---

## 🤖 Machine Learning Anomaly Detection Evaluation

The Isolation Forest algorithm was rigorously evaluated on a held-out test split of **1,200 telemetry samples** containing ground-truth failure injections (CPU cryptomining spikes, slow memory leaks, network DDoS bursts, cascading container failure).

| Metric | Measured Value | Operational Significance |
| :--- | :--- | :--- |
| **Precision** | **83.09%** | Very low false alarm rate during normal diurnal traffic |
| **Recall** | **99.12%** | Successfully caught 113 out of 114 real infrastructure failure events |
| **F1-Score** | **90.40%** | Optimal harmonic balance between sensitivity and precision |
| **ROC-AUC** | **0.9890** | Outstanding discriminatory separation between normal & anomalous states |

**Confusion Matrix (1,200 Benchmark Samples):**
- True Negatives (TN): **1,063**
- False Positives (FP): **23**
- False Negatives (FN): **1**
- True Positives (TP): **113**

---

## 🧪 Running Automated Tests

Execute the comprehensive backend test suite via `pytest`:

```bash
pytest backend/tests/test_api.py -v
```

**Test Results (13 / 13 passed - 100% success rate):**
```
backend/tests/test_api.py::test_root_endpoint PASSED                     [  7%]
backend/tests/test_api.py::test_healthcheck PASSED                       [ 15%]
backend/tests/test_api.py::test_prometheus_metrics PASSED                [ 23%]
backend/tests/test_api.py::test_security_jwt_and_hashing PASSED          [ 30%]
backend/tests/test_api.py::test_auth_login_flow PASSED                   [ 38%]
backend/tests/test_api.py::test_dashboard_overview PASSED                [ 46%]
backend/tests/test_api.py::test_live_metrics PASSED                      [ 53%]
backend/tests/test_api.py::test_container_management PASSED              [ 61%]
backend/tests/test_api.py::test_deployments_flow PASSED                  [ 69%]
backend/tests/test_api.py::test_alert_lifecycle_and_escalation PASSED    [ 76%]
backend/tests/test_api.py::test_incident_resolution PASSED               [ 84%]
backend/tests/test_api.py::test_controlled_self_healing_security PASSED  [ 92%]
backend/tests/test_api.py::test_reports_and_ml_evaluation PASSED         [100%]
====================== 13 passed in 14.59s =======================
```

---

## 📚 Academic & Viva Voce Documentation

Comprehensive documentation has been prepared inside the `docs/` folder:
- **`docs/PROJECT_REPORT.md`**: Complete 24-section formal academic project report.
- **`docs/CloudOps_Presentation.pptx`**: 12-slide executive presentation generated using `python-pptx`.
- **`docs/PRESENTATION_SLIDES.md`**: Slide-by-slide script and defense notes.
- **`docs/DEMONSTRATION_GUIDE.md`**: Step-by-step evaluator walkthrough covering all 10 demonstration phases.
- **`docs/VIVA_PREPARATION.md`**: 35 comprehensive viva questions and answers spanning Cloud, DevOps, Docker, Prometheus, FastAPI, ML, and Architecture.

---

## 🛡️ License

This project is licensed under the MIT License - open for educational and academic capstone evaluation.
