# 기초 플랫폼(환경 셋팅) 만들기

## 기초 플랫폼 흐름

Postgres 설치 (Airflow 메타DB + 향후 분석 DB까지 담을 수 있는 DB 서버)

MinIO 설치 (raw 저장소 / 외부스토리지)

Airflow 설치 (설치 과정에서 자동으로 airflow metadata 테이블 생성 = db init/migrate 수행)

접속 확인(포트포워드) + 정상 동작 체크

중요한 포인트: Postgres만 설치하면 airflow db init은 절대 안 됨.
Airflow를 설치할 때, Airflow chart가 migration job을 돌리면서 자동으로 테이블을 만든다.

## data-platform namespace 생성

```
kubectl create namespace data-platform
```

#### [네임스페이스란?](../../docs/name-space.md)

## StorageClass 확인

```
kubectl get storageclass
```

local-path가 보이면 OK

#### [StorageClass란?](../../docs/storage-class.md)

## PostgreSQL, MinIO, Airflow 설치

### Helm repo 추가

#### [Helm이란?](../../docs/helm.md)

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add apache-airflow https://airflow.apache.org
helm repo update
```

---

### Airflow 설치

설치:

```
helm repo add apache-airflow https://airflow.apache.org
helm upgrade --install airflow apache-airflow/airflow --namespace airflow --create-namespace
```

#### [공식문서](https://airflow.apache.org/docs/helm-chart/stable/index.html)

---

### MinIO 설치

내용:
MinIO는 외부스토리지로 사용할 예정으로 쿠버네티스의 관리 대상이 아님
단일 컨테이너로 올려서 관리

설치:

```yaml
services:
  minio:
    image: quay.io/minio/minio:RELEASE.2025-03-12T18-04-18Z
    container_name: minio
    ports:
      - "9900:9000"
      - "9901:9001"
    command: ["server", "--console-address", ":9001", "/data"]
    environment:
      MINIO_ROOT_USER: minioAccessKey
      MINIO_ROOT_PASSWORD: minioSecretKey
    volumes:
      - ../minio-data:/data
    restart: unless-stopped

  createbuckets:
    image: quay.io/minio/mc:RELEASE.2025-03-12T17-29-24Z
    depends_on:
      - minio
    restart: on-failure
    entrypoint: >
      /bin/sh -c "
      sleep 5;
      /usr/bin/mc alias set dockerminio http://minio:9000 minioAccessKey minioSecretKey;
      /usr/bin/mc mb dockerminio/raw;
      /usr/bin/mc mb dockerminio/processed;
      /usr/bin/mc mb dockerminio/archive;
      exit 0;
      "
```

```
docker compose up -d
```

---

---

#### 추가: 왜 yaml로 정의해서 설치할까?

쿠버네티스는 기본적으로 선언적 시스템임
내가 만드는 플랫폼 수준에서는 내장 Helm Chart를 사용해도 되지만
학습을 위해 Helm values.yaml로 정의하여 선언적 인프라 방식으로 구성함

기본 설치:

```

helm install [dbname] bitnami/postgresql

```

yaml 파일 정의 후 설치:

```

helm install postgres bitnami/postgresql -n data-platform -f values/postgres.yaml

```

잘 올라갔는지 확인:

```

kubectl get pods -n [내 namespace]

```

## 트러블 슈팅

1. bitnami repo를 찾기 못하는 상황 발생
   -> Helm은 사용자 단위로 repo 설정을 관리하기 때문에 sudo 사용 시 환경이 달라질 수 있다는 점을 경험했고, 이후부터는 sudo 없이 Helm을 사용하는 방식으로 정리했습니다.

2. 리눅스 계정에서 kubectl 명령어를 sudo(루트권한) 없이 사용 불가능
   -> sudo chmod 644 /etc/rancher/k3s/k3s.yaml 을 통해 일반 사용자도 읽기 권한 부여

3. MinIO를 외부 Object Storage로 쓰려는 목적이었는데 PV/PVC로 연결하려고 설계하면서 Kubernetes 스토리지 구조를 잘못 이해함.
   -> PV/PVC는 Kubernetes 내부 워크로드(PostgreSQL 등)의 파일/블록 스토리지를 위한 것이고, Object Storage(MinIO/S3)는 API(S3)로 직접 접근하는 외부 스토리지라는 점을 정리함. MinIO는 Kubernetes 밖에서 Docker Compose로 실행하고 로컬 디스크(`~/lab/minio-data`)를 컨테이너 `/data`에 볼륨 마운트하여 데이터를 저장하도록 구성함. 애플리케이션(Pod)은 PV를 통해 접근하는 것이 아니라 `http://<host>:9900` S3 endpoint와 AccessKey/SecretKey를 사용해 MinIO에 직접 접근하는 구조로 수정함.

```

```
