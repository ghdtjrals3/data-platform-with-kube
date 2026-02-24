# PV와 PVC (Kubernetes Storage 기본 개념)

Kubernetes에서 스토리지는 **Pod와 분리된 리소스**로 관리됩니다.  
그 핵심이 바로 **PV**와 **PVC**입니다.

---

## 1️⃣ PV (PersistentVolume)

> **실제 저장 공간(리소스)**

- 클러스터에 존재하는 실제 스토리지
- 관리자(또는 StorageClass)가 생성
- 디스크, NFS, 클라우드 볼륨 등을 표현

즉,  
👉 “실제로 존재하는 하드디스크 공간”

---

## 2️⃣ PVC (PersistentVolumeClaim)

> **저장 공간을 요청하는 요청서**

- 사용자가 생성
- 필요한 용량, 접근 모드 등을 명시
- 적절한 PV와 자동 매칭됨

즉,  
👉 “나 10Gi 필요해요”라고 요청하는 문서

---

## 3️⃣ 관계 구조
Pod → PVC → PV → 실제 디스크
- Pod는 PVC를 사용
- PVC는 PV와 바인딩
- PV는 실제 스토리지를 가리킴

---

## 4️⃣ 동작 흐름

### 📌 정적 프로비저닝 (예전 방식)

1. 관리자가 PV를 미리 생성
2. 사용자가 PVC 생성
3. 조건이 맞는 PV와 연결

---

### 📌 동적 프로비저닝 (현재 일반적 방식)

1. 사용자가 PVC 생성
2. StorageClass가 자동으로 PV 생성
3. PVC ↔ PV 바인딩
4. Pod에 마운트

---

## 5️⃣ 실제 예제

### 🔹 PVC 생성

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi