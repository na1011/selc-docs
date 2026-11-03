<!-- AI 입력팩 · K02/40 · SYS-JOB · 묶음 A · 기준일 2026-11-02 -->
# [K02/40] [SYS-JOB] 작업지시 체계 (Batch · Work · Task · Workflow · Action)

| 항목 | 내용 |
|---|---|
| 대분류 | 공통 — 시스템·공통·기준정보 (폴더 1_공통) |
| 입력 순서 | 2 / 40 (묶음 A: 공통·시스템) |
| 선행 키워드 | K01 SYS-ARC |
| 후행 키워드 | K03 SYS-UI, K04 SYS-ERR, K05 MST-CTR, K08 MST-OPT, K10 MST-ALC, K12 MST-EQP, K15 MST-SN, K17 INV-UNT, K19 INV-RSV, K20 EQP-VND, K30 OUB-BAT |
| 상태 | 초안(개념) · BR 7 · REQ 확정 8 / 확인필요 0 · 미결 2 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K01 SYS-ARC WCS 구성 (Planning · Execution)**: WCS 시스템 계층을 Planning과 Execution 둘로 나누고, 각각 별도 인스턴스로 분리한다. Planning은 SCP에 설치하고, Execution은 각 센터별 로컬에 설치한다.

## 머리말
> 분류: 업무=공통(작업지시) / 설비=전체 자동화 설비
> 관련 시스템: WCS, ECS | 관련 키워드: SYS-ARC, MST-SN, EQP-VND, MST-EQP, MST-OPT
> 상태: 초안(개념)

## 1. 개요
WCS는 프로세스별로 작업지시를 Batch → Work → Task의 계층으로 관리하고, Task는 WCS Planning이 생성하고, WCS Execution이 Task를 받아 Workflow에 따라 실제 설비 단위 작업지시인 Action을 생성한다(SYS-ARC). Action 이후 설비 인터페이스는 오픈솔루션의 통신표준화 솔루션이 처리한다. 시리얼 이력 로그의 변경 TASK번호는 이 체계의 Task를 가리킨다. 현재는 개념만 정의한 상태이며 세부(계층별 생성 기준·상태·번호 체계)는 추후 구체화한다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): Batch, EWMS, WCS, ECS, Work, Task, Action, W/T, 시리얼 이력 로그, 변경 TASK번호, Workflow, Planning, Execution, 통신표준화 솔루션, 전략 이력, 창고 기초속성, EWMS 수신 원본, PLANT, Valuation Type, 화주, LOT, LOT 생성 전략, STO, 직송, FO, 인도처, 사전 보충, 출고예정, 할당 전략, ORG DO, BOX Shuttle, 피킹호기, 보관호기, BIN, 팔레타이징, 3층 디팔레타이징 범위, GTP, 잔량, 재고실사, W/T 생성, 공팔레트 공급, 긴급 보충, GANTRY, 이동 W/T, 피킹 W/T, 입고대, 공팔레트 묶음, TRASH BIN, AS/RS, 수작업 영역

## 3. 프로세스 흐름
1. (WCS) 프로세스(입고·출고·이동 등)별 Batch 생성
2. (WCS) Batch 아래 Work 생성
3. (WCS Planning) Work 아래 Task 생성 → Execution에 전달
4. (WCS Execution) Task를 받아 Workflow에 따라 설비 단위 작업지시 Action 생성 → 통신표준화 솔루션을 거쳐 ECS로 전달
5. (ECS→WCS Execution) Action 실적 수신(통신표준화 솔루션 경유) → Workflow·Task 진행·완료 반영, 시리얼번호 단위 변경 시 변경 TASK번호로 시리얼 이력 로그 적재 (MST-SN)

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-SYS-JOB-001 | 작업지시 관리 | 프로세스별 Batch → Work → Task 계층으로 관리 |
| BR-SYS-JOB-002 | 설비 작업지시 | Task를 받아 Workflow에 따라 실제 설비 단위 작업지시(Action) 생성 |
| BR-SYS-JOB-003 | 시리얼 이력 추적 | 로그의 변경 TASK번호 = 이 체계의 Task 번호 |
| BR-SYS-JOB-004 | 생성 주체 | Task = WCS Planning, Action = WCS Execution이 Workflow에 따라 생성 (SYS-ARC) |
| BR-SYS-JOB-005 | 작업방식·전략 기록 | 모든 프로세스의 작업방식·전략은 작업지시(Batch·Work·Task) 단위로 전략 이력에 남긴다 (SYS-ARC BR-SYS-ARC-008) |
| BR-SYS-JOB-006 | Work 생성 | 출고예정·W/T를 인터페이스로 수신하면 원본을 그대로 별도 테이블에 보관하고(EWMS 수신 원본), WCS가 LOT 속성들을 포함한 가공 데이터를 만든다. 이 가공 데이터가 Work다 |
| BR-SYS-JOB-007 | Work 종류 | Work에는 W/T에 맵핑되어 실적을 반영하는 Work와, W/T와 무관하게 작업지시를 위해 생성한 Work가 함께 존재한다 |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-SYS-JOB-001 | WCS는 프로세스별 작업지시를 Batch, Work, Task 계층으로 관리해야 한다. | WCS | 확정 |
| REQ-SYS-JOB-002 | WCS는 Task를 받아 Workflow에 따라 실제 설비 단위 작업지시인 Action을 생성하여 ECS에 전달해야 한다 (Workflow 추가). | WCS | 확정 |
| REQ-SYS-JOB-003 | Task는 WCS Planning이 생성하고, Action은 WCS Execution이 Workflow에 따라 생성해야 한다 (SYS-ARC). | WCS | 확정 |
| REQ-SYS-JOB-004 | WCS는 Batch·Work·Task를 생성할 때 적용한 작업방식·전략(옵션 값, Workflow, 사용자 판단 포함)을 해당 작업지시와 연결된 전략 이력으로 남겨야 한다 (SYS-ARC REQ-SYS-ARC-011). | WCS | 확정 |
| REQ-SYS-JOB-005 | WCS는 출고예정·W/T 수신 시 원본을 별도 테이블에 그대로 보관하고, LOT 속성들을 포함한 가공 데이터(Work)를 만들어 이후 할당·작업지시에 써야 한다. | WCS | 확정 |
| REQ-SYS-JOB-006 | Work 가공 방식: ① 원본 연결 — Work 자체에는 원본 키(인터페이스 ID·문서 번호·행 번호·수신 PLANT)를 두지 않고, Work–원본 연결 테이블(Work ID, 인터페이스 ID, W/T·출고예정 번호, 행 번호, 수신 PLANT, 행별 지시 수량·실적 수량)로 여러 원본 행 → 한 Work(N:1)를 관리한다(MST-STK REQ-MST-STK-010). Work에 원본 키를 붙이면 행마다 Work가 갈라져 합칠 수 없기 때문이다 ② 화주 — PLANT–화주 맵핑을 이 시점에 적용해 확정(MST-STK REQ-MST-STK-004) ③ LOT 속성 — LOT 생성 전략에서 출고 체크를 켠 속성마다 기준값을 만든다: 원본에 해당 항목이 있으면 그 값(예: Valuation Type), 없으면 창고 기초속성의 기본값, 기본값도 없으면 '조건 없음'(그 속성은 비교하지 않음) ④ 할당 전략 적용 키 — 프로세스 유형·출고 유형(STO/직송)·시간대 Type·FO·인도처를 Work에 담아 할당 전략 선택(MST-ALC ①)에 쓰고, 직송 Work에는 할당한 GANTRY 호기도 담는다(EQP-GNT BR-EQP-GNT-035) ⑤ 통합 키 — 가공 속성이 같은 원본 행은 한 Work로 합친다. 통합 키 = 창고, 화주(맵핑 후), 제품, LOT 속성 기준값, 프로세스 유형, 출고 유형, 작업지시 단위(직송 FO / STO ORG DO), 인도처, 시간대 Type. 수신 PLANT는 통합 키에서 빼고 연결 테이블에만 두어 화주 맵핑으로 통합된 PLANT끼리 합칠 수 있게 한다(MST-STK BR-MST-STK-007). 실적은 연결 테이블의 원본 행에 정해진 순서대로 순차 반영해 W/T·수신 PLANT 단위로 보낸다(PARTIAL 포함, MST-STK BR-MST-STK-009, OUB-CFM BR-OUB-CFM-008) ⑥ 원본이 변경·취소되면 원본은 새 이력으로 쌓고 연결 행 단위로 반영한다: 진행 전 Work는 해당 행 수량을 빼거나 다시 가공하고, 진행 중 Work는 변경 대상으로 표시한다. | WCS | 확정 |
| REQ-SYS-JOB-007 | WCS는 W/T에 맵핑되어 실적을 반영하는 Work와 W/T와 무관하게 작업지시를 위해 생성하는 Work를 모두 관리하고, Work마다 어느 쪽인지 구분해야 한다. | WCS | 확정 |
| REQ-SYS-JOB-008 | Work 구분과 처리: ① Work 구분 코드 — W/T 맵핑 Work / W/T 무관 Work(WCS 생성) ② W/T 맵핑 Work — EWMS 피킹·이동 W/T 원본에서 가공, 원본 연결 테이블로 실적을 W/T·수신 PLANT 단위로 순차 반영(REQ-SYS-JOB-006, MST-STK BR-MST-STK-009) ③ W/T 무관 Work — 생성 근거(출고예정 기반 사전 보충, 긴급 보충, 보관호기 → 피킹호기 이동, 장기 보관 팔레타이징, BOX Shuttle 공간 확보, 공팔레트 공급·배출, 공팔레트 묶음·TRASH BIN 3층 입고대 투입(EQP-ASRS BR-EQP-ASRS-048), 재고실사 배출, 3층 부분 해체 재입고 등)와 근거 참조(출고예정 원본 행, 설비 요청 ID, 사용자 ID, 상위 Work)를 남기고, EWMS 실적은 보내지 않는다(재고 이력·전략 이력만) ④ BIN을 넘는 이동 — 층간 이동·수작업 영역에서 시작하는 이동(1층 → 3·4층 수작업 이동 INB-MAN, 출고확정 후 운반 등)은 EWMS가 생성해 보낸 W/T를 받아 W/T 맵핑 Work로 만든다(WCS의 W/T 생성 요청 없음, MST-LOC BR-MST-LOC-036). WCS 처리 결과로 BIN이 바뀌는 경우(GTP 잔량 OUB-MAN, W/T 없는 AS/RS 배출 EQP-ASRS REQ-EQP-ASRS-031 ⑥)는 이동 W/T 생성·확정을 함께 처리하고 그 Work를 W/T 맵핑으로 연결한다 ⑤ 화주 맵핑·LOT 속성 기준값·할당 전략 적용은 두 종류 모두 같게 한다 ⑥ 출고예정으로 만든 사전 보충 Work는 W/T 무관 Work이고, 이후 받은 피킹 W/T는 별도의 W/T 맵핑 Work로 만든다(사전 보충 BOX는 출고예정에 예약하지 않음, INV-RSV BR-INV-RSV-006). | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| WCS Execution→ECS (통신표준화 솔루션 경유) | Action 생성 시 | Action ID, Task 번호, 설비 ID, 작업 구분, 대상(팔레트/BOX), FROM/TO (항목 추후 구체화) |
| ECS→WCS Execution (통신표준화 솔루션 경유) | Action 실적 발생 시 | Action ID, Task 번호, 결과, 시각 (항목 추후 구체화) |

## 7. 예외 처리
- 추후 구체화

## 8. 미결 사항
- Q-193: [추후 정리] 출고 체크 속성 기준값으로 쓸 원본 항목(예: Valuation Type)의 실제 목록 — EWMS W/T·출고예정 항목표 확보 후 정리 (REQ-SYS-JOB-006·008 확정·사전 보충 Work 별도 해석 확정·기준값 순서 = 원본 항목 → 창고 기본값 → 조건 없음 결정)
- Q-081: [추후 정리] Batch·Work·Task·Workflow·Action 계층별 생성 기준(프로세스별), 상태 전이, 번호 체계, Task ↔ EWMS W/T 매핑, Action 실패·재지시 처리, 시리얼 이력 로그에 Action ID 함께 기록 여부

---
[SYS-JOB 끝 · 다음 입력: K03 SYS-UI 단말 역할 (PDA · PC · 대시보드)]
