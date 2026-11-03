<!-- AI 입력팩 · K07/40 · MST-STK · 묶음 B · 기준일 2026-11-02 -->
# [K07/40] [MST-STK] 재고관리체계 (PLANT · Storage Location · 화주 매핑)

| 항목 | 내용 |
|---|---|
| 대분류 | 공통 — 시스템·공통·기준정보 (폴더 1_공통) |
| 입력 순서 | 7 / 40 (묶음 B: 기준정보 — 조직·재고체계·거래처·할당 전략) |
| 선행 키워드 | K05 MST-CTR, K06 MST-OWN |
| 후행 키워드 | K10 MST-ALC, K11 MST-LOC, K14 MST-LOT, K37 INV-SYNC |
| 상태 | 일부확정 · BR 10 · REQ 확정 13 / 확인필요 1 · 미결 1 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K05 MST-CTR 창고(CDC) 마스터**: 창고(CDC)를 화주(MST-OWN)와 함께 WCS 관리의 최상위 단위로 관리한다. 로케이션·설비 등 기준정보와 재고·작업지시 관리는 창고 아래에, 제품·재고관리체계(화주·Storage Location 맵핑)는 화주 아래에 속한다.
- **K06 MST-OWN 화주(화주사) 마스터**: 화주(화주사)를 창고(CDC)와 함께 WCS 관리의 최상위 레벨로 관리한다. 제품은 화주사별로 관리한다.

## 머리말
> 분류: 업무=기준정보 / 설비=공통
> 관련 시스템: EWMS, WCS | 관련 키워드: MST-CTR, MST-OWN, MST-LOT, INV-SYNC, MST-LOC
> 상태: 일부확정

## 1. 개요
EWMS는 PLANT와 Storage Location 조합으로 Stock Type을 관리하고, WCS는 이를 화주·WCS Storage Location으로 맵핑하여 별도 관리한다. 맵핑 기준과 PLANT별 재고 혼용 정책을 정의한다. 평택 CDC도 화주를 구분하도록 기능을 구현하며, 재고를 통합할지 구분할지는 PLANT–센터코드–화주 맵핑 데이터로 정한다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): EWMS, WCS, PLANT, Storage Location, Stock Type, 화주, LOT, W/T, CDC, Work, Task, 창고코드, 재고대사, W/T 확정, 창고 기초속성, EWMS 수신 원본, 전략 이력, 적치 W/T, 재고 처리 표준 메소드, 파워유저, 출고예정, 통합 에러관리

## 3. 프로세스 흐름
1. (WCS) PLANT 관리 화면에서 PLANT코드별 WCS 맵핑 화주사코드·WCS 맵핑 창고코드 등록 (BR-MST-STK-010)
2. (EWMS) W/T에 PLANT, Storage Location 정보를 담아 WCS로 전송 → (WCS) 수신 데이터를 EWMS 수신 원본으로 그대로 보관
3. (WCS) Work 생성 시점에 맵핑을 WCS 기준으로 적용: PLANT → 화주, EWMS Storage Location → WCS Storage Location
4. (WCS) 이후 프로세스(LOT 채번 MST-LOT, 출고 할당 시 화주 체크)는 Work에 적용된 화주로 진행
5. (WCS→EWMS) 실적 송신 시 EWMS 수신 원본을 기준으로 원래 수신된 PLANT별로 실적을 나눠 전달

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-MST-STK-001 | PLANT = L175 또는 L1UU | WCS 화주 001로 맵핑, 보관재고 혼용 사용 (맵핑 데이터 예시, 최종 데이터는 사용자 정리 Q-014) |
| BR-MST-STK-002 | PLANT = L1E3 | WCS 화주 002로 맵핑, 별도 재고 관리 (맵핑 데이터 예시, 최종 데이터는 사용자 정리 Q-014) |
| BR-MST-STK-003 | EWMS Storage Location이 다르나 용도가 동일 | WCS 기준 Storage Location 하나로 맵핑 (예: L167 FC01 정상품, FC02 세트구성품창고, FC03 한총e-store → 모두 L167 FC01) |
| BR-MST-STK-004 | 화주 구분 기능 | 평택 CDC도 화주를 구분하도록 기능을 구현한다. 재고 통합 여부는 기능이 아니라 PLANT–화주 맵핑 데이터로 정한다(여러 PLANT를 한 화주로 맵핑하면 통합, 다른 화주로 맵핑하면 구분) |
| BR-MST-STK-005 | PLANT 관리·맵핑 적용 시점 | PLANT 관리를 두고 PLANT별 센터코드(창고)와 화주를 맵핑해 관리한다. 맵핑은 Work 생성 시점에 WCS 기준으로 적용하고, 이후 프로세스는 적용된 화주로 진행한다 |
| BR-MST-STK-006 | EWMS 창고 식별 | EWMS 센터코드와 WCS 창고코드는 항상 맵핑으로 연결하며(같은 코드 여부는 맵핑 등록자가 판단), W/T는 PLANT 기준이므로 창고는 PLANT–센터코드 맵핑으로 식별한다 (MST-CTR BR-MST-CTR-006) |
| BR-MST-STK-007 | EWMS 실적 송신 단위 | EWMS W/T는 PLANT 기준으로 관리된다. WCS에서는 PLANT 맵핑관계로 작업지시 데이터를 통합 가공할 수 있으므로, EWMS로 실적을 보낼 때는 원래 수신된 PLANT 기준으로 데이터를 나눠 전달한다 |
| BR-MST-STK-008 | 수신 데이터 보관 | EWMS와 WCS의 자료구조가 다르므로, 실적 송신에 쓰기 위해 EWMS에서 받은 데이터를 그대로 WCS에 보관한다(EWMS 수신 원본) |
| BR-MST-STK-009 | 통합 Work의 PARTIAL 실적 반영 | 여러 원본 W/T를 합친 Work가 일부만 처리(PARTIAL)되면, 실적 수량을 정해진 룰대로 원본 W/T에 순차적으로 채워 반영한다 |
| BR-MST-STK-010 | WCS PLANT 관리 | WCS에도 PLANT 관리를 둔다. 관리 항목은 PLANT코드, WCS 맵핑 화주사코드, WCS 맵핑 창고코드다(화주 예: L175 → 001, L1UU → 001, L1E3 → 002). EWMS W/T의 PLANT로 화주와 창고를 함께 정한다 (같은 날 창고코드 추가) |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-MST-STK-001 | WCS는 PLANT 마스터와 PLANT → 센터코드(창고)·화주 맵핑 마스터를 관리해야 한다. | WCS | 확정 |
| REQ-MST-STK-002 | WCS는 EWMS (PLANT, Storage Location) → WCS (PLANT, Storage Location) 맵핑 마스터를 관리해야 한다. | WCS | 확인필요 |
| REQ-MST-STK-003 | 서로 다른 화주(별도 재고 관리 PLANT)의 재고가 혼용되지 않도록 출고 할당 시 화주를 체크해야 한다. 평택 CDC에도 적용한다. | WCS | 확정 |
| REQ-MST-STK-004 | WCS는 Work 생성 시점에 PLANT–화주 맵핑을 WCS 기준으로 적용해 Work에 화주를 확정하고, 이후 LOT 채번·할당·출고는 그 화주로 처리해야 한다. | WCS | 확정 |
| REQ-MST-STK-005 | 맵핑 데이터가 바뀌어도 이미 생성된 Work·LOT·재고의 화주는 바꾸지 않는다. 변경된 맵핑은 이후 생성되는 Work부터 적용한다. 맵핑을 바꿀 때 영향받는 재고(바뀐 PLANT가 속했던 화주의 재고) 목록을 보여 주고, 파워유저가 화주 변경 재고 처리를 실행할 수 있어야 한다. 화주는 LOT 필수 속성이므로 LOT를 다시 채번하며 재고 처리 표준 메소드로 이력을 남긴다. EWMS 재고는 PLANT 기준이라 바뀌지 않으므로 EWMS 연계는 하지 않는다(맵핑만 바꾸면 기존 재고가 새 화주 Work에 할당되지 않아 출고가 막히므로). | WCS | 확정 |
| REQ-MST-STK-006 | PLANT 관리 부가 항목(REQ-MST-STK-013 보완): 적용 시작일, 사용 여부, 등록·수정 이력. 화주는 창고 아래가 아닌 공통 관리 대상이므로 PLANT 관리 화면은 공통 기준정보 화면으로 둔다. | WCS | 확정 |
| REQ-MST-STK-007 | WCS는 W/T 수신 시 받은 PLANT를 W/T(EWMS 수신 원본)에 보존하고(Work까지 보존할 필요 없음 — Work와는 원본 연결로 추적), 작업지시 데이터를 통합 가공해 처리했더라도 EWMS에 실적(W/T 확정 등)을 보낼 때는 수신된 PLANT 기준으로 나눠 전달해야 한다. | WCS | 확정 |
| REQ-MST-STK-008 | 실적 PLANT는 재고가 입고될 때의 PLANT가 아니라 해당 W/T에 수신된 PLANT를 따른다(예: L175로 입고된 통합 재고를 L1UU W/T로 출고하면 L1UU로 보고). 여러 PLANT의 W/T를 한 Work·Task로 묶어 처리한 경우 실적 수량은 정해진 순서대로 원본 W/T에 순차 반영하고(BR-MST-STK-009), 부족분은 W/T별로 표시한다. | WCS | 확정 |
| REQ-MST-STK-009 | WCS는 EWMS에서 받은 W/T 등 작업 데이터를 가공하지 않고 그대로 보관(EWMS 수신 원본)하고, EWMS 실적을 만들 때 이 원본을 기준으로 해야 한다. | WCS | 확정 |
| REQ-MST-STK-010 | EWMS 수신 원본은 Work–원본 연결 테이블(Work ID, 인터페이스 ID, W/T·출고예정 번호, 행 번호, 수신 PLANT, 행별 지시·실적 수량)로 WCS Work와 연결해(여러 원본 행 → 한 Work, N:1, SYS-JOB REQ-SYS-JOB-006 ①) 추적할 수 있어야 하며, 수신 일시·인터페이스 ID를 함께 보관한다. 보관 기간은 실적 송신 완료 후 일정 기간(창고 기초속성 MST-OPT 변수)으로 한다. | WCS | 확정 |
| REQ-MST-STK-011 | WCS는 통합 Work의 PARTIAL 실적을 정해진 순서 규칙에 따라 원본 W/T 행에 순차적으로 채워 W/T·수신 PLANT 단위 실적으로 반영해야 한다. | WCS | 확정 |
| REQ-MST-STK-012 | PARTIAL 순차 반영 순서 규칙: ① 시간대 Type 마감이 빠른 W/T ② 같은 마감이면 EWMS 수신 일시가 빠른 W/T ③ 같으면 W/T 번호·행 번호 순. 앞 행의 지시 수량을 모두 채운 뒤 다음 행으로 넘기고, 남은 행은 미처리 수량으로 표시한다. 규칙은 창고 기초속성(MST-OPT) 변수로 바꿀 수 있게 하고, 적용 결과는 전략 이력(SYS-ARC)에 남긴다. | WCS | 확정 |
| REQ-MST-STK-013 | WCS는 PLANT 관리 기능(화면)을 두어 PLANT코드, WCS 맵핑 화주사코드, WCS 맵핑 창고코드를 등록·수정해야 하며, EWMS 수신 시 이 맵핑으로 창고를 식별하고 Work 생성 시 화주를 정한다(REQ-MST-STK-004, MST-CTR REQ-MST-CTR-008). | WCS | 확정 |
| REQ-MST-STK-014 | EWMS에서 받은 W/T·오더·출고예정의 PLANT 또는 Storage Location이 맵핑 마스터(PLANT–센터코드–화주 맵핑)에 없으면 처리하지 않고 수신 건을 보류해 오류 이력으로 남기며, 통합 에러관리의 인터페이스 오류 분류로 알람한다(SYS-ERR REQ-SYS-ERR-004). 맵핑을 등록한 뒤 보류된 수신 건을 다시 처리할 수 있어야 한다(MST-CTR REQ-MST-CTR-006과 같은 방식). | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| EWMS→WCS | 적치 W/T 수신 시 | PLANT, Storage Location, Stock Type |
| WCS→EWMS | 실적 송신(W/T 확정 등) 시 | W/T 번호, 수신된 PLANT, Storage Location, 수량 (EWMS 수신 원본 기준, PLANT별 분리) |

## 7. 예외 처리
- 맵핑 마스터에 없는 PLANT/Storage Location 수신 시: 수신 건 보류·오류 이력·인터페이스 오류 알람, 맵핑 등록 후 재처리 (REQ-MST-STK-014)

## 8. 미결 사항
- Q-014: [추후 정리] PLANT–센터코드–화주 맵핑 데이터 (사용자가 데이터로 정리 예정, L1UU의 WCS 화주 포함) (평택도 화주 구분 기능 구현, 통합 여부는 맵핑 데이터로 결정)

---
[MST-STK 끝 · 다음 입력: K08 MST-OPT 창고 기초속성]
