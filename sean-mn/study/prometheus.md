# 프로메테우스 (Prometheus)란?

프로메테우스는 대상 시스템으로부터 각종 모니터링 지표를 수집하여 저장하고 검색할 수 있는 시스템이다.

서버, 애플리케이션, 데이터베이스 등 다양한 시스템의 상태를 메트릭(숫자 데이터) 형태로 수집하고 저장한다.

## 핵심 개념

### 메트릭

프로메테우스가 수집하는 데이터의 단위이다.
모든 메트릭은 아래 형태를 가진다.

```
metric_name{label1="value1", label2="value2"} 값 타임스탬프
```

메트릭의 타입은 4가지이다.

| 타입 | 설명 | 예시 |
|------|------|------|
| Counter | 단조 증가하는 값 | 총 요청 수, 에러 수 |
| Gauge | 증가/감소 모두 가능한 값 | CPU 사용률, 메모리 사용량 |
| Histogram | 관측값을 구간(bucket)별로 집계 | 응답 시간 분포 |
| Summary | 분위수(quantile) 기반 집계 | p50, p99 응답 시간 |
 

###  Pull 방식

프로메테우스는 데이터를 **직접 가져오는(Pull) 방식**을 사용한다.

```
Prometheus  →  (HTTP GET /metrics)  →  모니터링 대상
```

모니터링 대상이 데이터를 밀어넣는(Push) 방식이 아니라, 프로메테우스가 주기적으로 스크래핑(scraping)한다.

이 주기를 `scrape_interval`로 설정한다. (기본값: 15초)

> Push가 필요한 경우(배치 작업, 람다 등)에는 **Pushgateway**를 중간에 두는 방식을 사용한다.

### Exporter
 
모니터링 대상이 `/metrics` 엔드포인트를 직접 제공하지 못하는 경우, **Exporter**가 그 역할을 대신한다.
 
- `node_exporter` : 서버의 CPU, 메모리, 디스크 등 OS 레벨 메트릭
- `mysqld_exporter` : MySQL 상태 메트릭
- `blackbox_exporter` : HTTP, TCP 등 외부 엔드포인트 상태 체크


### PromQL
 
프로메테우스 전용 쿼리 언어다. 수집된 데이터를 조회하고 가공하는 데 사용한다.
 
```promql
# 최근 5분간 초당 HTTP 요청 수
rate(http_requests_total[5m])
 
# CPU 사용률 (%)
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
 
# 메모리 사용률 (%)
(1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes) * 100
```

### Alertmanager
 
프로메테우스 자체에서 알림 규칙을 정의하고, **Alertmanager**가 실제 알림을 발송한다.
 
```
Prometheus  →  (알림 조건 충족)  →  Alertmanager  →  Slack / Email / PagerDuty 등
```

## 정리
 
- 프로메테우스는 **Pull 방식**으로 메트릭을 수집하는 오픈소스 모니터링 시스템이다.
- 데이터는 **시계열 DB**에 저장되며, **PromQL**로 조회한다.
- 직접 메트릭을 노출하지 못하는 대상은 **Exporter**를 통해 연동한다.
- 알림은 **Alertmanager**가 담당하고, 시각화는 보통 **Grafana**와 함께 사용한다.
 