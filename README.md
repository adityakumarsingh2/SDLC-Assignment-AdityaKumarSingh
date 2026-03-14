# 📱 Smart-Attend — SDLC Assignment

> **Course:** SDLC & DevOps Fundamentals  
> **Project:** Smart-Attend — Geo-fencing Student Attendance App  
> **Role:** Junior Project Manager  

---

## 🌐 Live Demo

`index.html` is the dynamic view of `analysis.pdf`, deployed at:
**[https://adityakumarsingh2.github.io/SDLC-Assignment-AdityaKumarSingh/](https://adityakumarsingh2.github.io/SDLC-Assignment-AdityaKumarSingh/)**

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `index.html` | Dynamic web view of the analysis — live at the GitHub Pages URL above |
| `analysis.pdf` | Full project analysis report (Waterfall failure + Agile pivot + Sprint Backlog) |
| `sprint_backlog.csv` | Sprint 1 Backlog with user stories, priorities & estimates |
| `README.md` | Project overview and CI/CD rationale (this file) |

---

## 📋 Project Overview

**Smart-Attend** is a mobile app that uses **GPS Geo-fencing** to automatically mark student attendance when they enter a 10-metre radius around their classroom. Halfway through development, a new requirement was added by the Dean: **Biometric (Face ID) Verification** to prevent proxy attendance.

This assignment analyses:
1. Why a **Waterfall model** would fail under this mid-project pivot
2. How switching to the **Scrum (Agile) framework** solves the problem
3. A practical **Sprint Backlog** for Sprint 1 (MVP)

---

## 🔄 Why CI/CD is Used to Push Sprint Updates to Students

### What is CI/CD?

**Continuous Integration (CI)** and **Continuous Deployment (CD)** are DevOps practices that automate the process of testing and releasing software changes. In the context of Smart-Attend:

- **CI** — Every time a developer pushes code (e.g., completing US03 geo-fencing logic), the CI pipeline automatically **builds the app and runs all unit tests** to ensure nothing is broken.
- **CD** — Once CI passes, the CD pipeline automatically **packages and deploys the new app version** (e.g., to the Google Play Store Beta / Apple TestFlight) so students and teachers receive the update without manual team effort.

---

### 🚀 How CI/CD Serves Our Scrum Sprints

In a **Scrum framework**, we deliver a working product increment at the end of **every 2-week Sprint**. Without CI/CD, deploying this update would require:
- Manual build compilation (risk of "works on my machine" errors)
- Manual upload to Play Store / App Store (takes 1–3 days for review)
- Manual notification to students and teachers

With CI/CD, this entire pipeline is **automated**:

```
Developer pushes code
        ↓
CI Pipeline (GitHub Actions)
  ├── Run unit tests
  ├── Run integration tests (geo-fence + login)
  ├── Build Android APK / iOS IPA
  └── Report pass/fail to team
        ↓
CD Pipeline (on merge to main)
  ├── Deploy to Firebase App Distribution (internal testing)
  ├── Promote to Play Store Beta / TestFlight
  └── Send update notification to registered students
```

---

### 📦 Sprint-by-Sprint CI/CD Flow

#### Sprint 1 → MVP Release
- US01 (Login), US02 (Teacher Dashboard), US03 (Geo-fence), US04 (Notifications), US05 (Admin Override) are merged into `main`
- CI pipeline runs all tests → CD automatically deploys the MVP to TestFlight for beta testers (students + teachers)
- Students receive the app **within minutes of Sprint completion**, not weeks

#### Sprint 2 → Biometric Update
- Face ID integration is merged — CI runs regression tests to ensure Sprint 1 features are unbroken
- CD pushes the updated build to all existing beta users automatically
- No manual "release day" — students see the update in their app notification tray

---

### 🛡️ Key CI/CD Benefits for Smart-Attend

| Benefit | How It Helps Smart-Attend |
|---------|--------------------------|
| **Automated Testing** | Catches geo-fence/Face ID integration bugs before reaching students |
| **Fast Delivery** | Sprint updates reach students on the same day the Sprint ends |
| **Rollback Safety** | If Face ID breaks geo-fencing, CD can auto-rollback to Sprint 1 build |
| **Transparency** | Dean & stakeholders can see live build status and changelogs |
| **Reduced Manual Effort** | Developers focus on features, not manual deployments |
| **Consistent Quality** | Every release passes the same test suite automatically |

---

### 🛠️ Suggested CI/CD Toolchain

```yaml
# .github/workflows/smart-attend-ci.yml (example)

name: Smart-Attend CI/CD Pipeline

on:
  push:
    branches: [main, sprint-1, sprint-2]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Flutter / React Native
        uses: subosito/flutter-action@v2

      - name: Install Dependencies
        run: flutter pub get

      - name: Run Unit Tests
        run: flutter test

      - name: Build APK
        run: flutter build apk --release

  deploy-to-testflight:
    needs: build-and-test
    runs-on: macos-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to Firebase App Distribution
        run: firebase appdistribution:distribute build/app-release.apk
             --app $FIREBASE_APP_ID
             --groups "beta-students,teachers"
```

---

### 💡 Summary

> CI/CD is the **DevOps backbone** that makes Scrum's promise of "working software every Sprint" a reality. Without it, each Sprint ends in a manual, error-prone release process. With CI/CD, every merge to `main` automatically tests, builds, and deploys the latest Smart-Attend features to students — achieving **true Continuous Delivery** aligned with the Agile manifesto's goal of *customer satisfaction through early and continuous delivery of valuable software*.

---

*Assignment submitted as part of SDLC & DevOps Fundamentals coursework.*
