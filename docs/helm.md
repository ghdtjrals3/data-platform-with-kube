# Helm 기본 설명 (Kubernetes 패키지 매니저)

Helm은 Kubernetes 리소스(YAML)를 **차트(Chart)** 라는 패키지로 묶어 **설치/업그레이드/롤백/삭제** 를 쉽게 해주는 도구입니다.  
앱 배포에 필요한 Deployment, Service, Ingress, ConfigMap 등을 “템플릿 + 값(values)” 구조로 관리합니다.

---

## Helm에서 자주 나오는 용어

- **Chart**: 배포 단위 패키지(예: `myapp` 차트).  
  템플릿(`templates/`)과 기본 설정(`values.yaml`)을 포함합니다.
- **Release**: 차트를 실제 클러스터에 설치한 “인스턴스”.  
  같은 Chart도 release 이름을 바꾸면 여러 번 설치 가능.
- **Repository**: 차트를 모아두는 저장소(예: Bitnami repo).

---

## Helm을 쓰는 이유

- YAML을 직접 복붙/수정하는 대신, **변수화(values)** 해서 재사용 가능
- `helm upgrade`로 **버전 관리/업데이트**가 쉬움
- `helm rollback`으로 **롤백**이 쉬움
- `--set`/`-f values-*.yaml`로 환경(dev/stg/prod) 분리 가능

---

## 자주 쓰는 Helm 명령어

```bash
# 차트 생성(스캐폴딩)
helm create myapp

# 설치(Release 생성)
helm install myapp-release ./myapp -n myns --create-namespace

# 업그레이드(값 변경 포함)
helm upgrade myapp-release ./myapp -n myns -f values.yaml

# 현재 적용될 YAML 미리보기(렌더링)
helm template myapp-release ./myapp -n myns

# 설정값 확인
helm get values myapp-release -n myns

# 배포 이력 확인
helm history myapp-release -n myns

# 롤백
helm rollback myapp-release 1 -n myns

# 삭제
helm uninstall myapp-release -n myns