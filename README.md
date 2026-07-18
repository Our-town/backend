# 우리 동네 어때요? - Backend

위치 기반 주거지 추천 서비스 "우리 동네 어때요?"의 백엔드 레포지토리입니다.
직장(또는 학교) 위치와 예산, 원하는 조건을 입력하면 통근 시간·생활 인프라를 기준으로 매물을 추천하고, 지역별 점수를 히트맵으로 보여줍니다.

## 기술 스택

- Java 17, Spring Boot 3.5, Spring Data JPA
- PostgreSQL, Flyway
- Gradle
- 외부 API: 서울시 부동산 전월세 정보 API, 카카오맵 API, ODsay 대중교통 API, TMAP API
- Python (AI 점수 산출 모듈 연동, ProcessBuilder로 실행)

## 주요 기능

- **매물 데이터 수집/관리**: 서울시 공공 API에서 전월세 매물 데이터를 주기적으로 적재하고, 좌표 변환·정제 처리
- **통근 시간 계산**: 출발지-목적지 간 대중교통/차량 이동 시간을 조회
- **매물 추천**: 지역, 예산, 통근 시간 조건에 맞는 매물을 필터링해 추천 (일반/페이지네이션 방식 지원)
- **지역 히트맵**: 안전, 주거환경, 녹지, 교통, 의료, 편의시설 가중치를 반영한 지역별 점수 산출

## API

| Method | Endpoint | 설명 |
| --- | --- | --- |
| GET | `/api/rent/all` | 전체 매물 조회 |
| GET | `/api/rent/gu` | 구 단위 매물 조회 |
| GET | `/api/rent/dong` | 동 단위 매물 조회 |
| GET | `/api/rent/deposit` | 보증금 범위로 매물 검색 |
| GET | `/api/rent/rentFee` | 월세 범위로 매물 검색 |
| POST | `/api/rent/fix-coordinates` | 좌표 없는 매물 좌표 변환 |
| GET | `/api/rent/loadAll` | 서울시 API에서 최근 1년치 데이터 적재 |
| GET | `/api/rent/update-now` | 최신 계약 정보 수동 갱신 |
| DELETE | `/api/rent` | 전체 매물 삭제 |
| GET | `/api/commute/transit-detail` | 출발지-목적지 대중교통 상세 경로 조회 |
| POST | `/api/recommendation` | 조건 기반 매물 추천 |
| POST | `/api/recommendation/paged` | 조건 기반 매물 추천 (페이지네이션) |
| POST | `/api/heatmap` | 가중치 기반 지역 히트맵 점수 조회 |

## 실행 방법

1. `src/main/resources/application.properties.example`을 참고해 `application.properties` 생성 후 API 키, DB 접속 정보 입력
2. PostgreSQL DB 준비
3. 아래 명령으로 서버 실행

```bash
./gradlew bootRun
```

## 참고

AI 팀이 구현한 점수 산출/백분위 계산 스크립트(`ai_module`)는 협업 규칙에 따라 공개 레포에서 제외되어 있으며, 백엔드는 로컬 환경에서 해당 스크립트를 실행해 결과를 받아옵니다.
