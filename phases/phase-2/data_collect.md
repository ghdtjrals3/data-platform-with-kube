# 데이터 수집

## 데이터 수집 흐름

## 다른 Provider 추가
현재 helm으로 기본 설치만 한 상태 다음 명령어를 실행해서
다른 Provider 추가

```
예시> S3
helm get values airflow -n airflow -o yaml > airflow-values.yaml

cat > airflow-values.yaml << 'EOF'
env:
  - name: _PIP_ADDITIONAL_REQUIREMENTS
    value: "apache-airflow-providers-amazon>=8.0.0 apache-airflow-providers-postgres>=5.0.0"
EOF

helm upgrade airflow apache-airflow/airflow \
  -n airflow \
  -f airflow-values.yaml
```
## Airflow - minIO 연결
Airflow - minIO 연결을 위해서 Airflow conections 에 다음과 같은 내용을 추가
```
커넥션 유형: Amazon Web Services
AWS Access Key ID: [yaml에서 정의한 내 Key ID]
AWS Secret Access Key: [yaml에서 정의한 내 Secret Access Key]
추가필드 (region_name도 필수로 들어가야됨)
{
  "endpoint_url": "http://192.168.19.112:9900",
  "region_name": "us-east-1"
}
```
