# MUST Campus Academic Resource Sharing Platform

> 校园学术资源共享平台 — 课程全栈系统（曾部署，当前演示已下线）  
> System Analysis and Design · Group F · Spring 2026  
> School of Business · Macau University of Science and Technology

---

## 🌐 Deployment Status

The VPS demo has been taken offline. The previous demo URL and hosted API documentation are no longer available. Source code and local setup instructions remain in this repository.

VPS 演示已下线，原演示地址及在线 API 文档已停止提供；源码与本地运行说明仍保留在本仓库。

---

## 👥 Team

- 连宇翔 (1230020693) — Project Lead, System Architecture, Database, Backend, Deployment
- 郁凯杰 (1230020426) — System Architecture, Database
- 陈瀚中 (1230032209) — Frontend, Prototype, UI

**Lecturer:** Dr. CHE Pak Hou (Howard)

---

## 🎯 Two Core Features (per BBAZ16604 requirement)

### Feature 1 — *Improvement*: Optimized Precise Retrieval
Improves on the existing fragmented academic resource discovery experience that students currently rely on (WeChat group searches, paid third-party platforms, peer-to-peer messaging from upperclassmen).

- Multi-dimensional filtering: Course Code · Academic Year · Resource Type · Min Rating
- Composite relevance ranking: 40% match accuracy + 30% download popularity + 30% average rating
- Preview snippets to reduce mistaken downloads
- Measured search response < 100 ms

### Feature 2 — *New Feature*: Points-Based Incentive System
A new feature not present on any existing campus resource platform.

- Welcome bonus: 100 points on registration
- Earn: upload approval +10, download received +2, rating received +1
- Spend: download cost -1 (with 3 free daily downloads as a floor)
- Redeem: 50 pts → 10 extra downloads · 100 pts → 7-day resource pin
- Atomic transactions: SELECT ... FOR UPDATE row-level locking

---

## 📂 Repository Structure

```
.
├── README.md
├── .gitignore
│
├── Prototype.html                 # Interactive front-end (single-file, calls real API)
│
├── backend/                       # FastAPI backend (previously deployed on VPS)
│   ├── app/                         # Application source code
│   │   ├── main.py                    # FastAPI entry point
│   │   ├── config.py                  # Configuration (constants, weights)
│   │   ├── database.py                # SQLAlchemy connection pool
│   │   ├── models.py                  # ORM models (8 tables)
│   │   ├── schemas.py                 # Pydantic request/response models
│   │   ├── auth.py                    # JWT + bcrypt authentication
│   │   ├── points_engine.py           # Atomic points transaction logic
│   │   ├── search_engine.py           # Composite relevance ranking
│   │   ├── seed.py                    # Initial data seed script
│   │   └── routers/                   # 5 API route modules
│   │       ├── auth.py                  # /api/auth/*
│   │       ├── resources.py             # /api/resources/*
│   │       ├── points.py                # /api/points/*
│   │       ├── ratings.py               # /api/ratings/*
│   │       └── admin.py                 # /api/admin/*
│   │
│   ├── schema.sql                   # MySQL/MariaDB DDL
│   ├── Dockerfile                   # Container image
│   ├── docker-compose.yml           # MariaDB + API orchestration
│   ├── nginx_campus.conf            # Reverse-proxy config
│   ├── requirements.txt             # Python dependencies
│   ├── deploy.sh                    # One-shot deployment script
│   ├── upload_to_vps.sh             # Local-to-VPS sync helper
│   ├── README.md                    # Backend operations guide
│   └── .env.example                 # Environment variable template
│
└── scripts/                       # Documentation & PPT generation scripts
    ├── gen_requirements.py
    ├── gen_system_design.py
    ├── gen_progress_reports.py
    ├── gen_prototype_spec.py
    ├── gen_test_report.py
    ├── gen_final_report.py
    ├── gen_speech_script.py
    ├── gen_presentation.py
    └── patch_charter.py
```

> **Note:** Project deliverables (`.docx` reports, `.pptx` presentation, course resources) are submitted to Moodle and intentionally excluded from this public repository. The `scripts/` folder contains the Python tooling that generates them.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Single-file HTML + vanilla JS (no framework) |
| Backend | FastAPI (Python 3.11) + SQLAlchemy ORM |
| Database | MariaDB 10 (MySQL-compatible, schema.sql) |
| Authentication | JWT (`python-jose`) + bcrypt |
| Deployment | Docker Compose + nginx + Cloudflare Tunnel |
| Previous hosting | 1 GB RAM VPS · AlmaLinux 9 · demo taken offline |

---

## 🚀 Run Locally

```bash
git clone https://github.com/a2318491287-design/must-campus-resource-platform.git
cd must-campus-resource-platform/backend

# Install dependencies (Python 3.11+)
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Run with SQLite (zero-setup)
mkdir -p storage
DATABASE_URL="sqlite:///./local.db" \
STORAGE_DIR="$(pwd)/storage" \
python -m app.seed

DATABASE_URL="sqlite:///./local.db" \
STORAGE_DIR="$(pwd)/storage" \
uvicorn app.main:app --reload

# Open Prototype.html in your browser → it auto-detects localhost:8000
```

For full Docker / VPS deployment, see [`backend/README.md`](backend/README.md).

---

## 📊 Performance (previous VPS deployment)

Previously reported load-test results for the VPS deployment:

| Metric | Result |
|---|---|
| 100 concurrent requests | 100% success (2× expected peak) |
| Search p95 latency | 87 ms (vs 2,000 ms target) |
| 1000 atomic point deductions | 0 race conditions |
| Memory footprint | 166 MB / 340 MB cap |

---

## 📋 Project Management

This repo serves as project management evidence per the BBAZ16604 requirement to *"use software for managing the group"*:

- **Closed Issues** (8 phase-based tasks, all completed) — Issues tab
- **Commit history** — full version history of code & design changes
- **README + structured codebase** — complete project navigation

---

## 📝 License

Academic project for educational purposes. Course materials referenced in the project (lecture slides, textbook excerpts) belong to their original copyright holders and are not included in this repository.
