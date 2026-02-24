# 🚀 K3s 기반 기본형 데이터 플랫폼

> 한 프로젝트로 끝까지 가는 Kubernetes 실전 로드맵  
> k3s 위에서 데이터 수집부터 시각화까지 직접 설계하고 구축합니다.

---

## 📌 프로젝트 소개

이 프로젝트는 단순한 Kubernetes 실습이 아닙니다.

**“운영 가능한 기본형 데이터 플랫폼”을 하나 끝까지 완성하는 것**이 목표입니다.

데이터 파이프라인을 다음 흐름으로 직접 구현합니다:

Web/API 수집 → 정제(ETL) → 저장 → 검색/지표 → 시각화


그리고 모든 컴포넌트는 k3s(Kubernetes 경량 배포판) 위에 배포합니다.

---

## 🎯 프로젝트 목적

이 프로젝트를 통해 다음을 경험합니다:

- Kubernetes 기반 서비스 설계
- 컨테이너 기반 데이터 파이프라인 구축
- 데이터 저장/검색 구조 설계
- 실무형 아키텍처 이해
- 끝까지 완성하는 프로젝트 경험

---

## 🏗 전체 아키텍처 개요

### 1️⃣ Web/API Collector
- 외부 API 또는 웹 데이터 수집
- Kubernetes Deployment 또는 CronJob 형태로 실행

### 2️⃣ ETL / 정제 계층
- 데이터 정제 및 가공
- Kubernetes Job 기반 처리

### 3️⃣ 저장 계층
- PostgreSQL (정형 데이터 저장)
- Object Storage (원본 데이터 저장)
- Elasticsearch (검색/지표용)

### 4️⃣ 지표 / 검색 API
- 내부 통계 API 서버 구성
- 검색/집계 기능 제공

### 5️⃣ 시각화
- Tableau 연결
- 대시보드 구성

---

## 🧱 기술 스택

- Kubernetes (k3s)
- Docker
- Python
- PostgreSQL
- Elasticsearch
- Tableau

---

## 📂 프로젝트 구조
├── collector/ # 데이터 수집기
├── etl/ # 정제 로직
├── api/ # 지표 제공 API
├── infra/ # Kubernetes 리소스
├── docs/ # 개념 및 심화 설명
└── README.md

---

## 🗺 단계별 로드맵

### 1️⃣ 플랫폼 기초 구축
- k3s 설치
- kubectl 설정
- Namespace 구성
- 기본 네트워크 이해

### 2️⃣ 데이터 수집기 배포
- Python 수집기 작성
- Docker 이미지 빌드
- Deployment 또는 CronJob 구성

### 3️⃣ ETL 구성
- 정제 로직 작성
- Kubernetes Job 설계

### 4️⃣ 데이터 저장소 구축
- PostgreSQL 배포
- Elasticsearch 구성

### 5️⃣ 지표 API 구성
- REST API 서버 배포
- 내부 서비스 연결

### 6️⃣ 시각화 연동
- Tableau 연결
- 기본 대시보드 구성

---

## 🧠 프로젝트 철학

- ❌ 과도한 마이크로서비스 분리 금지
- ❌ 초반부터 복잡한 도구 도입 금지
- ✅ 작게 시작해서 점진적으로 확장
- ✅ 실제 운영 가능한 구조 유지
- ✅ 개념과 실습을 함께 정리

---

## 📘 문서 전략

README는 전체 흐름을 설명합니다.

세부 개념은 `docs/` 폴더에서 관리합니다.

예시:

- Namespace 개념 정리
- Ingress 설명
- Deployment vs Job 차이
- Service 타입 정리
- 데이터베이스 구조 설계 이유

---

## 🏁 최종 목표

✔ k3s 클러스터에서  
✔ 데이터가 자동 수집되고  
✔ 정제되어 저장되며  
✔ 검색 가능하고  
✔ Tableau 대시보드로 시각화된다  

그리고 이 모든 구조를 **설계 이유까지 설명할 수 있는 상태**가 된다.

