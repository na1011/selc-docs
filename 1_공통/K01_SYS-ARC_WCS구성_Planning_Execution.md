<!-- AI 입력팩 · K01/40 · SYS-ARC · 묶음 A · 기준일 2026-11-02 -->
# [K01/40] [SYS-ARC] WCS 구성 (Planning · Execution)

| 항목 | 내용 |
|---|---|
| 대분류 | 공통 — 시스템·공통·기준정보 (폴더 1_공통) |
| 입력 순서 | 1 / 40 (묶음 A: 공통·시스템) |
| 선행 키워드 | 없음 |
| 후행 키워드 | K02 SYS-JOB, K03 SYS-UI, K10 MST-ALC, K19 INV-RSV, K20 EQP-VND, K25 EQP-TRB, K33 OUB-MAN, K37 INV-SYNC, K38 INV-CNT |
| 상태 | 일부확정 · BR 10 · REQ 확정 13 / 확인필요 4 · 미결 2 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- 없음 (첫 순서이거나 앞 순서 관련 키워드 없음)

## 머리말
> 분류: 업무=공통(시스템 구성) / 설비=전체 자동화 설비
> 관련 시스템: WCS, ECS, 통신표준화 솔루션 | 관련 키워드: SYS-JOB, EQP-VND, INV-SYNC, SYS-UI, OUB-MAN
> 상태: 일부확정

## 1. 개요
WCS 시스템 계층을 Planning과 Execution 둘로 나누고, 각각 별도 인스턴스로 분리한다. Planning은 SCP에 설치하고, Execution은 각 센터별 로컬에 설치한다. Planning은 Task를 생성하고, Execution은 Task를 받아 Workflow에 의해 설비 작업지시용 Action을 만든다. Action 이후 설비와의 인터페이스는 오픈솔루션의 통신표준화 솔루션이 처리한다(해당 접점은 현재 설계 중). 둘을 분리하면 Planning과 Execution 사이에 접점이 생기므로, 네트워크 단절 시에도 각각 독립적으로 작업을 수행할 수 있는 방안을 검토한다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): EWMS, WCS, ECS, Batch, Work, Task, Workflow, Planning, Execution, 통신표준화 솔루션, 재고 스냅샷, W/T, 시리얼 이력 로그, Action, 재고대사, AMR, 유인지게차, GTP, EWMS PDA, 팔레트 유형, 재고 이동 로그, 출고 DOCK, 전략 이력, 창고 기초속성, 3층 디팔레타이징 범위, 재할당, 로딩 옵티마이저, 사전 보충, SCP, 창고코드, WCS PDA, SDS, 수직반송기, 수작업 영역, 팔레트 작업 상태, 셀 상태, 팔레트갈이, 설비 기준정보, 대시보드

## 3. 프로세스 흐름
1. (WCS Planning) EWMS 오더·W/T와 기준정보를 바탕으로 Task 생성 (SYS-JOB)
2. (Planning→Execution) Task 전달 — Planning·Execution 접점
3. (WCS Execution) Task를 받아 Workflow에 따라 설비 작업지시용 Action 생성
4. (Execution→통신표준화 솔루션→ECS) Action 전달 — 접점 설계 중
5. (ECS→통신표준화 솔루션→Execution) 설비 실적 수신 → Action 단위로 취합 (관리 방식 REQ-SYS-ARC-006 추천)
5-1. (Execution) WCS 작업 상태와 수신 신호(설비 실적, 수직반송기 출발·도착 신호, AMR 결과, WCS PDA 스캔)를 통합해 팔레트 작업 상태(현재 위치·작업 상태·다음 위치)를 갱신 → Workflow 다음 단계 진행 (REQ-SYS-ARC-015·016)
6. (Execution→Planning) Workflow·Task 진행·완료 보고 — Planning·Execution 접점

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-SYS-ARC-001 | WCS 구성 | Planning과 Execution으로 분리하고 각각의 역할을 나눈다 |
| BR-SYS-ARC-002 | Planning 역할 | Task를 생성한다 |
| BR-SYS-ARC-003 | Execution 역할 | Task를 받아 Workflow에 의해 설비 작업지시용 Action을 만든다 |
| BR-SYS-ARC-004 | Action 이후 설비 인터페이스 | 오픈솔루션의 통신표준화 솔루션이 처리한다. 해당 접점은 현재 설계 중 |
| BR-SYS-ARC-005 | 설비 실적 | Action에 데이터를 취합해 관리할 수 있다. 더 나은 방안이 있으면 추천 반영 |
| BR-SYS-ARC-006 | 네트워크 단절 | Planning과 Execution이 각각 독립적으로 작업을 수행하는 방안을 검토한다. 분리로 Planning·Execution 접점이 생긴다 |
| BR-SYS-ARC-007 | 팔레트 단위 흐름 Workflow 설정 | 팔레트 단위 흐름(이송 단계와 단계별 수단)은 그때그때 달라지므로 Workflow를 설정할 수 있게 하고, 설정된 대로 작업이 이어지도록 구성한다 (예: GTP 출고 팔레트 1층 도착 후 출고 DOCK까지 AMR 또는 유인지게차, OUB-MAN) |
| BR-SYS-ARC-008 | 작업방식·전략 이력 | 모든 프로세스의 작업방식이나 전략은 이력으로 남겨야 한다 |
| BR-SYS-ARC-009 | 인스턴스 분리·설치 위치 | Planning과 Execution은 각자 별도 인스턴스로 분리한다. Planning은 SCP에 설치하고, Execution은 각 센터별 로컬에 설치한다 |
| BR-SYS-ARC-010 | 팔레트 작업 상태 | WCS 작업 상태와 수신 신호 정보를 통합해 팔레트 작업 상태를 정리·추론한다. 작업지시 대상(설비 ECS, AMR, 수직반송기)과 WCS PDA 스캔 단계를 모두 포함한다 |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-SYS-ARC-001 | WCS는 Planning과 Execution으로 분리해 구성하고, 각각의 역할을 나눠야 한다. | WCS | 확정 |
| REQ-SYS-ARC-002 | WCS Planning은 Task를 생성해 Execution에 전달해야 한다. | WCS Planning | 확정 |
| REQ-SYS-ARC-003 | WCS Execution은 Task를 받아 Workflow에 따라 설비 작업지시용 Action을 생성해야 한다. | WCS Execution | 확정 |
| REQ-SYS-ARC-004 | Action 이후 설비와의 인터페이스는 오픈솔루션의 통신표준화 솔루션을 통해 처리해야 한다 (접점 설계 중). | WCS Execution, 통신표준화 솔루션 | 확정 |
| REQ-SYS-ARC-005 | 네트워크 단절 시에도 Planning과 Execution이 각각 독립적으로 작업을 수행할 수 있는 방안을 검토·설계해야 한다. | WCS | 확정 |
| REQ-SYS-ARC-006 | 설비 실적 관리 — Action 레코드에 실적을 덮어쓰지 않고, Action ID를 키로 한 실적 이벤트(수신 원문·결과·시각, 추가만 하는 이력)로 쌓는다. Action 상태는 이벤트로 갱신하고, Workflow·Task 상태는 하위 Action 상태를 집계해 정한다. 이렇게 하면 부분 실적·재시도·중복 수신을 추적할 수 있고, 시리얼 이력 로그(MST-SN)와 재고 이력의 근거로 그대로 쓸 수 있다. | WCS Execution | 확인필요 |
| REQ-SYS-ARC-007 | 네트워크 단절 대응 — Execution은 이미 받은 Task 범위 안에서 Workflow·Action을 계속 수행하고, 이를 위한 기준정보·재고 사본을 로컬에 둔다. Task 진행·실적 보고는 로컬 대기열에 쌓았다가 복구 시 이벤트 ID 순서대로 재전송하고, 받는 쪽은 같은 이벤트 ID를 중복 처리하지 않는다. Planning은 단절 중 신규 Task를 대기열에 보관한다. 복구 후 재고 차이는 스냅샷 재고대사(INV-SYNC)로 확인한다. | WCS | 확인필요 |
| REQ-SYS-ARC-008 | Planning·Execution 접점은 Task 전달, Task·Workflow 진행·실적 보고, 기준정보 동기화, 재고 변동 통보로 정의하고, 메시지마다 메시지 ID·순번·생성 시각을 두어 재전송·순서 보장 규칙을 정해야 한다. | WCS | 확인필요 |
| REQ-SYS-ARC-009 | WCS는 팔레트 단위 흐름을 Workflow로 설정하는 기능을 제공하고, Execution은 설정된 Workflow의 단계 순서대로 다음 작업을 이어서 생성·지시해야 한다. 단계별 수행 수단(설비, AMR, 유인지게차 등)은 Workflow 설정으로 바꿀 수 있어야 한다. | WCS | 확정 |
| REQ-SYS-ARC-010 | Workflow 설정 항목: 적용 조건(창고, 작업 유형, 출발·도착 존·로케이션, 팔레트 유형), 단계 순서, 단계별 수행 수단(설비·AMR·유인지게차·수작업), 단계 완료 조건(설비 실적 / PDA 스캔), 대체 단계(예: AMR 불가 시 유인지게차), 사용 여부·적용 시작일. 설정 변경은 진행 중 Task에 영향을 주지 않고 이후 시작하는 Task부터 적용하며(MST-OPT REQ-MST-OPT-005와 같은 원칙), 유인지게차·수작업 단계는 WCS 작업지시 없이 PDA 완료 스캔으로 위치를 이동 이력에 반영한다. | WCS | 확정 |
| REQ-SYS-ARC-011 | WCS는 모든 프로세스에서 작업에 적용한 작업방식·전략을 전략 이력으로 남겨야 한다. | WCS | 확정 |
| REQ-SYS-ARC-012 | 전략 이력 항목: Batch·Work·Task ID, 프로세스, 적용 전략·작업방식(예: 할당 우선순위, 사전 보충 전략, 3층 디팔레타이징 범위, 로딩 옵티마이저 수행 주체, 재할당 사유), 적용 Workflow ID·버전, 참조한 창고 기초속성 변수와 그 시점 값, 결정 주체(시스템 / 사용자)와 사용자 판단 시 사용자 ID·사유, 적용 시각. 생성 시점에 남기고 수정하지 않으며(추가만), 전략이 바뀌면 새 이력을 추가한다. 전략 비교 지표(OUB-GNT REQ-OUB-GNT-012)와 부족 이력 집계에 이 이력을 쓴다. | WCS | 확정 |
| REQ-SYS-ARC-013 | WCS Planning과 Execution은 별도 인스턴스로 구성해야 한다. Planning은 SCP에 설치하고, Execution은 각 센터(창고)별 로컬에 설치한다. | WCS | 확정 |
| REQ-SYS-ARC-014 | 배치에 따른 역할 분담: ① Planning(SCP) 하나가 여러 센터의 Execution을 상대하며, Task와 메시지에 창고코드를 넣어 센터별 Execution으로 보낸다 ② EWMS·SDS 등 상위·외부 인터페이스는 Planning이, 설비 ECS·WCS PDA·현장 PC 등 현장 인터페이스는 해당 센터 Execution이 맡는다 ③ 센터 운영에 필요한 기준정보(창고 기초속성, 설비, 로케이션, Workflow 설정)와 재고 사본은 Planning에서 관리하고 Execution 로컬에 동기화한다 ④ SCP–센터 간 통신이 끊기면 REQ-SYS-ARC-007 방식으로 Execution이 독립 수행한다. | WCS | 확인필요 |
| REQ-SYS-ARC-015 | WCS는 팔레트별 작업 진행 상태를 WCS 작업 상태(Task·Workflow·Action)와 수신 신호(설비 실적, 수직반송기 출발·도착 신호, AMR 결과, WCS PDA 스캔)를 통합해 정리하고, 이를 근거로 현재 위치·작업 상태·다음 위치를 추론해 제공해야 한다(SYS-UI REQ-SYS-UI-015). | WCS Execution | 확정 |
| REQ-SYS-ARC-016 | 팔레트 작업 상태 통합 모델: ① 근거 — Action 상태(생성·전송·수신 확인·진행·완료·실패·취소)와 수신 신호 이벤트(설비 실적, 수직반송기 출발·도착 센싱, AMR 출발·완료, WCS PDA 스캔)를 Action ID에 묶어 쌓는다(REQ-SYS-ARC-006 실적 이벤트). 작업지시가 없는 유인지게차 단계는 PDA 스캔(도착 스캔 필수, 출발 스캔 선택)을 신호로 본다 ② 작업 상태 추론 — 대기(현재 단계 Action 미생성·미전송) / 지시됨(Action 전송·수신 확인, 출발 신호 전) / 이송 중(출발 신호 또는 진행 실적 수신, 도착 신호 전) / 도착(도착 신호 수신, 다음 단계 Action 생성 전) / 완료(Workflow 마지막 단계 완료: 적치 완료·출고확정 등) / 보류(사용자 보류·셀 상태) / 오류(실패 실적, 도착 지연, 신호 불일치) ③ 현재 위치 — 마지막 수신 신호의 위치(출발 → 이송 중 셀, 도착 → 도착 포트·셀). WCS 재고 팔레트는 재고 위치(INV-UNT)와 같아야 하며 다르면 오류로 표시한다. WCS 재고가 아닌 이동(1층 → 3·4층 수작업 이동, 수작업 영역 운반)은 작업 기준으로만 관리한다 ④ 다음 위치 — Workflow 다음 단계의 목적지. 팔레트갈이 대상이면 바뀐 경로의 다음 단계(OUB-TRF REQ-OUB-TRF-009) ⑤ 신호 불일치 — 출발 신호 없이 도착 신호가 오면 출발도 함께 처리하고 이력에 표시 / 같은 신호 중복은 이벤트 ID로 한 번만 반영 / 작업지시가 없는 팔레트의 신호는 미지시 신호로 기록하고 알람 / 출발 후 기준 시간 안에 도착이 없으면 오류(기준 시간은 설비 마스터의 설비별 도착 지연 기준 시간, MST-EQP BR-MST-EQP-028) ⑥ 적용 범위 — 1층에 한정하지 않고 Workflow로 이어지는 모든 팔레트 이동 | WCS Execution | 확정 |
| REQ-SYS-ARC-017 | 전략 이력 조회 화면(PC 1개): ① 조회 조건 — 기간, 프로세스, 전략 종류, 결정 주체(시스템/사용자), Task ID ② 결과 — 이력 목록(REQ-SYS-ARC-012 항목)에서 Task 진행 이력(Task → Workflow → Action)으로 연결, 엑셀 내려받기 ③ 팔레트 작업 추적 화면(SYS-UI REQ-SYS-UI-016)에서 해당 Task의 전략 이력으로 바로 이동 ④ 전략별 지표 대시보드는 두지 않는다(운영 데이터 축적 후 검토). | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| WCS Planning→WCS Execution | Task 생성 시 | Task 정보 (항목 미정) |
| WCS Execution→WCS Planning | Workflow·Task 진행·완료 시 | Task 번호, 진행 상태, 실적 (항목 미정) |
| WCS Execution→통신표준화 솔루션→ECS | Action 생성 시 | Action 정보 (접점 설계 중, Q-111) |
| ECS→통신표준화 솔루션→WCS Execution | 설비 실적 발생 시 | Action ID, 실적 (접점 설계 중, Q-111) |

## 7. 예외 처리
- 네트워크 단절: 각각 독립 수행 방안 검토 (REQ-SYS-ARC-005·007)

## 8. 미결 사항
- Q-110: [추후 정리] Planning·Execution 상세 — Batch·Work 생성 주체(Planning 포함 여부), 단절 시 독립 수행 범위(어떤 작업까지 계속할지), 추천 항목(REQ-SYS-ARC-006~008·014: 실적 이벤트, 단절 대응, 접점 메시지, 인스턴스별 역할 분담) 채택 여부, SCP 정식 명칭·SCP–센터 간 네트워크 구성 (물리 배치: Planning = SCP, Execution = 센터별 로컬 결정)
- Q-111: [추가 협의] 오픈솔루션 통신표준화 솔루션과의 접점 설계 — Action·실적 전문 항목, 전달 방식, 장애 시 처리 (현재 설계 중)

---
[SYS-ARC 끝 · 다음 입력: K02 SYS-JOB 작업지시 체계 (Batch · Work · Task · Workflow · Action)]
