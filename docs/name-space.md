# 📦 Kubernetes Namespace

## 1️⃣ Namespace란 무엇인가?

Kubernetes에서 **Namespace**는 클러스터 내부 리소스를 **논리적으로 분리하는 가상 공간**이다.

하나의 Kubernetes 클러스터 안에서 여러 팀, 여러 환경, 여러 서비스를 운영할 수 있도록 리소스를 구분해준다.

쉽게 말하면:

> “하나의 큰 Kubernetes 안에 여러 개의 작은 작업 공간을 만드는 기능”

---

## 2️⃣ 왜 Namespace가 필요한가?

Kubernetes 클러스터는 기본적으로 하나의 큰 리소스 풀이다.  
Namespace 없이 사용하면 다음 문제가 발생한다:

- 리소스 이름 충돌
- 환경(dev/staging/prod) 구분 어려움
- 팀 간 리소스 간섭
- 권한 관리 복잡

Namespace는 이런 문제를 해결한다.

---

## 3️⃣ Namespace의 역할

### ✅ 1. 리소스 논리적 분리

같은 이름의 리소스도 Namespace가 다르면 공존 가능하다.

예:

dev/api-server  
prod/api-server

---

### ✅ 2. 환경 분리

일반적인 구성:

- dev
- staging
- prod

각 환경을 Namespace로 나누는 것이 일반적이다.

---

### ✅ 3. 팀 단위 분리

- team-a
- team-b
- data-platform

팀별로 Namespace를 나누면 충돌 없이 운영 가능하다.

---

### ✅ 4. 권한 관리 (RBAC)

Namespace 단위로 접근 권한을 제어할 수 있다.

예:
- 특정 팀은 dev Namespace만 접근 가능
- 운영팀은 prod 접근 가능

---

### ✅ 5. 리소스 제한

Namespace별로 CPU/Memory 사용량 제한 가능:

- ResourceQuota
- LimitRange

---

## 4️⃣ 기본 Namespace

Kubernetes에는 기본적으로 다음 Namespace가 존재한다:

| Namespace | 설명 |
|-----------|------|
| default | 기본 작업 공간 |
| kube-system | Kubernetes 시스템 컴포넌트 |
| kube-public | 모든 사용자가 읽을 수 있는 리소스 |
| kube-node-lease | 노드 상태 관리 |

확인 명령어:

kubectl get ns

---

## 5️⃣ Namespace 생성 및 관리

### 생성

kubectl create namespace data-platform

또는

kubectl create ns data-platform

---

### 조회

kubectl get namespaces

---

### 특정 Namespace 리소스 조회

kubectl get pods -n data-platform

---

### 삭제

kubectl delete namespace data-platform

⚠ Namespace를 삭제하면 내부 리소스도 모두 삭제된다.

---

## 6️⃣ YAML로 Namespace 정의하기

apiVersion: v1  
kind: Namespace  
metadata:  
  name: data-platform  

적용:

kubectl apply -f namespace.yaml

---

## 7️⃣ Namespace와 Cluster 범위 리소스 차이

Namespace에 속하는 리소스:

- Pod
- Deployment
- Service
- ConfigMap
- Secret

Cluster 범위 리소스:

- Node
- PersistentVolume
- ClusterRole
- CustomResourceDefinition

---

## 8️⃣ 우리 프로젝트에서의 Namespace 전략

이 프로젝트에서는 다음과 같이 구성할 예정이다:

data-platform

추후 확장 시:

data-dev  
data-staging  
data-prod  

초기에는 복잡하게 나누지 않고, 하나의 Namespace에서 시작한다.

원칙: 처음부터 과하게 쪼개지 않는다.

---

## 9️⃣ 정리

Namespace는:

- Kubernetes 클러스터 안의 논리적 경계
- 환경 분리 도구
- 팀 분리 도구
- 권한 관리 단위
- 리소스 제한 단위

운영 환경에서는 필수 개념이며,  
프로덕션 수준의 Kubernetes 운영에서는 반드시 설계 단계에서 고려해야 한다.