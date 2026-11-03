<!-- AI 입력팩 · K05/40 · MST-CTR · 묶음 B · 기준일 2026-11-02 -->
# [K05/40] [MST-CTR] 창고(CDC) 마스터

| 항목 | 내용 |
|---|---|
| 대분류 | 공통 — 시스템·공통·기준정보 (폴더 1_공통) |
| 입력 순서 | 5 / 40 (묶음 B: 기준정보 — 조직·재고체계·거래처·할당 전략) |
| 선행 키워드 | K02 SYS-JOB |
| 후행 키워드 | K06 MST-OWN, K07 MST-STK, K08 MST-OPT, K09 MST-CUS, K11 MST-LOC, K12 MST-EQP, K13 MST-PRD, K14 MST-LOT, K16 MST-ORD, K17 INV-UNT, K36 INV-QRY, K37 INV-SYNC |
| 상태 | 일부확정 · BR 6 · REQ 확정 8 / 확인필요 0 · 미결 0 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K02 SYS-JOB 작업지시 체계 (Batch · Work · Task · Workflow · Action)**: WCS는 프로세스별로 작업지시를 Batch → Work → Task의 계층으로 관리하고, Task는 WCS Planning이 생성하고, WCS Execution이 Task를 받아 Workflow에 따라 실제 설비 단위 작업지시인 Action을 생성한다(SYS-ARC). Action 이후 설비 인터페이스는 오픈솔루션의 통신표준화 솔루션이 처리한다.

## 머리말
> 분류: 업무=기준정보 / 설비=공통
> 관련 시스템: WCS, EWMS | 관련 키워드: MST-CUS, MST-OPT, MST-OWN, MST-LOC, MST-EQP, MST-STK, MST-LOT, MST-PRD, MST-ORD, SYS-JOB, INV-SYNC
> 상태: 일부확정

## 1. 개요
창고(CDC)를 화주(MST-OWN)와 함께 WCS 관리의 최상위 단위로 관리한다. 로케이션·설비 등 기준정보와 재고·작업지시 관리는 창고 아래에, 제품·재고관리체계(화주·Storage Location 맵핑)는 화주 아래에 속한다. 현재 대상 창고는 평택 CDC 하나이며, 추가되는 창고에 대비해 창고 관리 화면을 개발한다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): EWMS, WCS, CDC, Batch, Work, Task, Action, W/T, 창고 마스터, PLANT, Storage Location, 화주, LOT, TC, 시간대별 출고 Type, 창고코드, 창고 기초속성, 거래처, 통합 에러관리

## 3. 프로세스 흐름
관리 계층:
| 레벨 | 대상 | 관련 키워드 |
| --- | --- | --- |
| 1 | 창고(CDC) · 화주(화주사) | MST-CTR, MST-OWN |
| 2 | 창고 소속 기준정보 — 창고 기초속성, 존·로케이션·셀, 설비, 오더 유형 / 화주 소속 기준정보 — 제품, 재고관리체계(PLANT·Storage Location 맵핑) | MST-OPT, MST-LOC, MST-EQP, MST-ORD / MST-PRD, MST-STK |
| 공통 | 창고 아래에 두지 않는 공통 관리 대상 — 화주, 거래처 | MST-OWN, MST-CUS |
| 3 | LOT · 재고 | MST-LOT, INV-WT, INV-SYNC, INV-CNT, INV-MVT |
| 4 | 작업지시 — Batch · Work · Task · Action | SYS-JOB |

1. (WCS) 창고 관리 화면에서 창고 마스터 등록 (창고 추가 시에도 화면 등록)
2. (WCS) 하위 기준정보를 창고에 소속시켜 등록
3. (EWMS→WCS) 오더·W/T 수신 → PLANT로 PLANT–센터코드 맵핑(MST-STK)을 조회해 창고 식별 → 해당 창고의 기준정보로 처리

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-MST-CTR-001 | 관리 계층 | 창고(CDC)와 화주를 모든 관리의 최상위 레벨로 둔다. 기준정보·재고·작업지시는 창고 아래에, 제품은 화주 아래에 속한다 (화주 최상위 추가) |
| BR-MST-CTR-002 | LOT 채번 | 창고정보가 다르면 LOT를 따로 채번한다 (BR-MST-LOT-001) |
| BR-MST-CTR-003 | 하위 데이터 등록 | 기준정보·재고·작업 데이터는 창고코드를 키로 포함한다 |
| BR-MST-CTR-004 | 창고 추가 | 창고 마스터는 창고 관리 화면에서 등록·수정하며, 추가되는 창고도 화면으로 등록해 대비한다 |
| BR-MST-CTR-005 | 공통 관리 대상 | 거래처(MST-CUS)와 화주(MST-OWN)는 공통으로 관리하며 창고 아래 관리 대상에서 제외한다 |
| BR-MST-CTR-006 | 창고코드·창고 식별 | EWMS 센터코드와 WCS 창고코드는 항상 맵핑으로 연결한다. 같은 코드를 쓸지는 맵핑 정보를 등록하는 사용자가 판단한다. EWMS W/T는 PLANT 기준으로 관리되므로, WCS는 PLANT–센터코드 맵핑(MST-STK BR-MST-STK-005)으로 창고를 식별한다. 이 맵핑은 WCS PLANT 관리의 WCS 맵핑 창고코드로 등록한다(MST-STK BR-MST-STK-010) (같은 날 센터코드 공용 → 항상 맵핑으로 변경) |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-MST-CTR-001 | WCS는 창고(CDC) 마스터를 관리해야 한다. | WCS | 확정 |
| REQ-MST-CTR-002 | WCS는 창고를 화주와 함께 모든 기준정보·재고·작업지시 관리의 상위 레벨로 두어, 창고 소속 데이터가 반드시 하나의 창고에 속하도록 관리해야 한다. | WCS | 확정 |
| REQ-MST-CTR-003 | 창고 마스터 속성: 창고코드, 창고명, 주소, 운영 여부(사용/미사용), EWMS 창고 식별값 매핑(항상 사용, REQ-MST-CTR-008), 등록·수정 이력. 출고 마감 시각은 출고 마감시간 마스터(MST-ORD REQ-MST-ORD-010)에서 관리하고, TC센터는 거래처(MST-CUS)로 관리하므로 창고 유형·운영 시간은 두지 않는다. | WCS | 확정 |
| REQ-MST-CTR-004 | 창고 소속 기준정보(로케이션, 설비 등)와 재고·작업 데이터는 창고코드를, 화주 소속 기준정보(제품, PLANT·Storage Location 맵핑)는 화주 코드를 키로 포함해야 한다. | WCS | 확정 |
| REQ-MST-CTR-005 | WCS 사용자 권한과 화면 조회 범위를 창고 단위로 제한할 수 있어야 한다 (다중 창고 확장 대비). | WCS | 확정 |
| REQ-MST-CTR-006 | EWMS에서 받은 오더·W/T의 창고가 미등록이거나 운영 중이 아니면 처리하지 않고 오류 이력으로 남기며, 통합 에러관리의 인터페이스 오류 분류로 알람한다(SYS-ERR REQ-SYS-ERR-004). 창고·맵핑을 등록한 뒤 보류된 수신 건을 다시 처리할 수 있어야 한다. | WCS | 확정 |
| REQ-MST-CTR-007 | WCS는 창고 관리 화면을 개발해 창고 마스터를 등록·수정할 수 있게 하고, 추가되는 창고를 개발 변경 없이 화면 등록으로 운영에 포함할 수 있도록 대비해야 한다. | WCS | 확정 |
| REQ-MST-CTR-008 | WCS는 EWMS 센터코드와 WCS 창고코드를 코드 동일 여부와 관계없이 항상 맵핑(창고 마스터의 EWMS 창고 식별값 매핑)으로 변환해야 한다. 같은 코드로 둘지는 맵핑 정보를 등록하는 사용자가 정한다. EWMS에서 받은 오더·W/T의 창고는 PLANT–센터코드 맵핑(MST-STK REQ-MST-STK-001)으로 식별한다. | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| EWMS→WCS | 오더·W/T 수신 시 | PLANT (→ PLANT–센터코드 맵핑으로 창고 식별, EWMS 센터코드 → WCS 창고코드는 항상 맵핑) |

## 7. 예외 처리
- 미등록·비운영 창고 데이터 수신 시: 오류 이력·인터페이스 오류 알람, 등록 후 재처리 (REQ-MST-CTR-006)

## 8. 미결 사항

---
[MST-CTR 끝 · 다음 입력: K06 MST-OWN 화주(화주사) 마스터]
