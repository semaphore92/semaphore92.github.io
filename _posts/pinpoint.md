---
title: "Pinpoint APM 도입을 통한 효율적인 시스템 관리"
date: 2023-06-30 21:13:16 +0900
categories:
  - blog
tags:
  - Pinpoint
  - APM
 
---

현재 운영 중인 시스템에는 APM(Application Performance Management) 도구가 없다.
이로 인해 트래픽이 몰리는 시간대에 서버 지연 혹은 장애가 발생하면 이데 대한 상세한 원인 분석이 어려운 상황이었다.

위와 같은 문제를 개선하기 위한 여러 방안 중, 퍼포먼스 트래킹에 강점을 가진 Pinpoint를 도입하기로 결정하였다.


## 왜 Pinpoint APM을 선택했는가?

시스템에 적합한 APM 도구를 선택하기 위해 다음과 같은 기준을 설정하였다.


1. EndPoint를 쉽게 추적할 수 있고 이를 시각화할 수 있는가?
2. 서버의 트래픽, 응답시간, SQL 등 구체적인 추적이 가능한가?

이러한 요구사항을 만족하는 APM 중에서 Kibana와 Elasticsearch 조합을 고려하였으나, 상대적으로 적용이 복잡하여
손쉬운 적용을 위해 Pinpoint를 선택하였다.


## Pinpoint APM이란?

Pinpoint는 대규모 분산 시스템의 성능 문제를 해결하기 위해 개발된 APM 도구입니다. 사용자의 요청을 추적하고, 그 결과를 시각화하여 애플리케이션의 병목을 파악하고 최적화하는데 유용하다.

## Pinpoint 구조

Pinpoint는 크게 세 가지 구성 요소로 이루어져 있다.

- Pinpoint Agent: 애플리케이션 서버에 설치되어, 실시간 트랜잭션 데이터를 수집
- Pinpoint Collector: Agent로부터 수집된 데이터를 처리하고 HBase 데이터베이스에 저장
- Pinpoint Web: Collector로부터 수집된 데이터를 사용자에게 시각적으로 제공

```
java -jar -javaagent:/service/apm/pinpoint-agent-2.4.2/pinpoint-bootstrap-2.4.2.jar -Dpinpoint.applicationName=dev -Dpinpoint.config=/service/apm/pinpoint-agent-2.4.2/pinpoint-root.config -Dspring.profiles.active=dev /service/app.jar
```


### 시스템 환경

현재 OpenJdk 11 버전을 사용중이지만 Hbase 1.0 버전 사용을 위하여
jdk 1.8.0 버전을 별도 설치하였다.





설치버전
 - hbase-1.4.9
 - pinpoint-1.4.9
 - jdk-1.8.0

```
```

