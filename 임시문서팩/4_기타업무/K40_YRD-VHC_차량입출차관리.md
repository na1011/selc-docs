<!-- AI 입력팩 · K40/40 · YRD-VHC · 묶음 I · 기준일 2026-11-02 -->
# [K40/40] [YRD-VHC] 차량 입출차 관리 (야드 관리)

| 항목 | 내용 |
|---|---|
| 대분류 | 기타업무 — 재고 운영·차량·야드 (폴더 4_기타업무) |
| 입력 순서 | 40 / 40 (묶음 I: 차량·야드) |
| 선행 키워드 | K16 MST-ORD, K20 EQP-VND, K26 INB-PLT, K28 INB-MAN, K35 OUB-CFM |
| 후행 키워드 | 없음 |
| 상태 | 일부확정 · BR 16 · REQ 확정 19 / 확인필요 0 · 미결 6 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K16 MST-ORD 입출고 오더 유형**: EWMS 입고 오더 유형(해외생산·국내생산·국내OEM·해외OEM, 쉽백은 WCS 프로세스 제외), 출고 오더 유형(STO/직송), 출고 정보 유형(출고예정/피킹 W/T/시간대별 Type)과 각 유형이 WCS 처리 경로에 주는 영향을 정의한다. 출고 작업지시 단위는 출고 유형별로 다르다: 직송은 SO→DO→FO로 만들어 FO 단위로, STO는 동일 TC 기준 총량피킹이 가능하도록 DO를 ORG DO로 묶어 OR…
- **K20 EQP-VND 설비사 맵핑 · 설비 접점**: 자동화 설비별 설비사를 정의하고, 설비사가 다른 설비 사이에서 BOX/팔레트가 인계되는 설비 접점의 발생 위치를 정리한다. 설비사별로 ECS가 따로 있으므로 설비 접점에서는 설비끼리 직접 연동하지 않고 WCS가 중간 처리한다.
- **K26 INB-PLT 팔레트 입고 (일반입고 / 대형)**: 1층에서 하역 또는 입고 수작업 영역에서 수작업 팔레타이징한 팔레트를 1층 AS/RS에 투입한다. 투입 시점에 WCS가 EWMS 내역을 확인하여 AS/RS 보관을 결정하며, AS/RS 용량이 부족하면 3층 출고대로 배출한다(용량 부족 배출, Bypass와 별개). 1층 입고대 Reject는 Bypass와 구분하며, ECS가 판단해 에러 코드를 포함해 3층 출고대로 배출한다.
- **K28 INB-MAN 팔레트 3층 수작업 이동**: 1층에서 하역 또는 수작업 팔레타이징한 팔레트(국내OEM·해외OEM 등, 쉽백은 WCS 프로세스 대상 아니나 3층 수작업 이동 작업지시 필요 여부 협의 중 — MST-ORD Q-165)를 3층·4층으로 이동한다. W/T상 행선지가 3층인 입고가 대상이다.
- **K35 OUB-CFM 자동화 설비 출고완료 확정**: 자동화 설비에서 출고된 팔레트의 피킹 W/T를 확정하는 방식을 정의한다. 출고확정 시점은 수작업 영역으로 인계하는 건(GTP·직송 이적)은 인계 시점(3·4층 전달 완료), 나머지는 설비 Flow의 끝 부분이다.

## 머리말
> 분류: 업무=차량·야드 / 설비=입고 도크(1층 하역장), 출고 DOCK, ATL, 게이트 관리 시스템, PDA
> 관련 시스템: EWMS, FIS, 아마노시스템, WCS | 관련 키워드: MST-ORD, INB-PLT, INB-MAN, OUB-CFM, EQP-VND
> 상태: 일부확정

## 1. 개요
CDC에 들어오는 차량의 입차 → DOCK IN → DOCK OUT → 출차 상태를 관리하는 야드 관리 프로세스다. 입고 차량은 1층 하역장(입고 도크)에서 하차하고, 출고 차량은 출고 DOCK에서 상차한다. 입고 차량 입문계획은 EWMS(국내생산, 국내OEM)가 push하고 역수입 건(해외생산, 해외OEM)은 FIS가 push하며, 출고 차량정보는 모두 EWMS에서 받는다. 입차·출차는 게이트 관리 시스템(아마노시스템)에서 연동받고, 입문하면 WCS가 EWMS에 입문처리를 요청해 응답으로 받은 DOCK 지정 결과를 아마노시스템에 전송하고, DOCK IN·DOCK OUT는 키오스크 또는 PDA로 등록한다. WCS는 입문·DOCK IN·DOCK OUT·출문을 모두 EWMS에 통보한다. WCS는 DOCK IN/OUT 현황판을 제공한다. 차량은 차량번호로 찾는다. DOCK 기준정보는 사용자가 WCS에 등록하며 EWMS와 같은 데이터로 유지한다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): EWMS, WCS, FIS, CDC, ATL, 국내생산 입고, OEM생산 입고, 쉽백, FO, 야드 관리, 입차, 출차, 하역장, 출고 DOCK, 게이트 관리 시스템, EWMS PDA, 입고예정정보, 차량 입문계획, 국내OEM 입고, 해외OEM 입고, 역수입, W/T, DOCK IN, DOCK OUT, 도크관리, 창고 기초속성, WCS PDA, ECS, 생기연, 키오스크, DOCK 기준정보, 피킹 W/T

## 3. 프로세스 흐름
차량 상태 (입고·출고 공통):
| 순서 | 상태 | 의미 | 현장 표기 | 정보 출처 |
| --- | --- | --- | --- | --- |
| 1 | 입차 (Gate-In) | 차량이 CDC 정문을 통과해 구내에 진입 | 입문 | 게이트 관리 시스템(아마노시스템) 연동 |
| 2 | DOCK IN (Docking) | 차량이 지정 도크에 붙어 상·하차 대기 | DOCK IN | WCS PDA 또는 키오스크 등록, ATL DOCK은 센싱 체크 가능 |
| 3 | DOCK OUT (Loading/Unloading Complete) | 하차(입고) 또는 상차(출고) 작업 종료 | DOCK OUT | WCS PDA 또는 키오스크 등록, ATL DOCK은 센싱 체크 가능 |
| 4 | 출차 (Gate-Out) | 차량이 도크를 떠나 CDC 정문을 통과해 퇴장 | 출문 | 게이트 관리 시스템(아마노시스템) 연동 |

입고 차량 흐름:
1. 차량 입문계획 수신(push) — 국내생산·국내OEM = EWMS→WCS, 역수입(해외생산·해외OEM) = FIS→WCS (BR-YRD-VHC-009, 협의 Q-177)
2. (아마노시스템→WCS) 입문(입차) 정보 송신
3. (WCS→EWMS) 수신한 입문 정보로 입문처리 요청 → (EWMS→WCS) 응답으로 DOCK 지정 결과 수신
4. (WCS→아마노시스템) 지정 DOCK 정보 전송
5. (WCS PDA 또는 키오스크→WCS, ATL DOCK은 센싱 가능) 1층 하역장 도크에서 DOCK IN 등록 → (WCS→EWMS) DOCK IN 통보
6. 하차 (ATL 또는 수작업) → 입고 유형별 경로로 투입 (MST-ORD)
7. (WCS PDA 또는 키오스크→WCS, ATL DOCK은 센싱 가능) 하차 후 DOCK OUT 등록 → (WCS→EWMS) DOCK OUT 통보
8. (아마노시스템→WCS) 출문(출차) 정보 송신 → (WCS→EWMS) 출문 정보 통보

출고 차량 흐름:
1. (EWMS→WCS) 출고 차량(FO) 정보 수신
2. (아마노시스템→WCS) 입문(입차) 정보 송신
3. (WCS→EWMS) 수신한 입문 정보로 입문처리 요청 → (EWMS→WCS) 응답으로 DOCK 지정 결과 수신
4. (WCS→아마노시스템) 지정 DOCK 정보 전송
5. (WCS PDA 또는 키오스크→WCS, ATL DOCK은 센싱 가능) 출고 DOCK에서 DOCK IN 등록 → (WCS→EWMS) DOCK IN 통보
6. 상차 (ATL 또는 수작업)
7. (WCS PDA 또는 키오스크→WCS, ATL DOCK은 센싱 가능) 상차 후 DOCK OUT 등록 → (WCS→EWMS) DOCK OUT 통보
8. (아마노시스템→WCS) 출문(출차) 정보 송신 → (WCS→EWMS) 출문 정보 통보

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-YRD-VHC-001 | 차량 상태 관리 | 입차 → DOCK IN → DOCK OUT → 출차 순서로 상태 전환 (입고·출고 공통) |
| BR-YRD-VHC-002 | 입고 유형 = 국내생산 / 국내OEM | 차량 입문계획 원천 = EWMS |
| BR-YRD-VHC-003 | 입고 유형 = 해외생산 / 해외OEM (역수입) | 차량 입문계획 원천 = FIS |
| BR-YRD-VHC-004 | 입차·출차 | 게이트 관리 시스템(아마노시스템)에서 연동 수신 |
| BR-YRD-VHC-005 | DOCK IN·DOCK OUT | 차량이 도크에 붙으면 WCS PDA 또는 키오스크로 DOCK IN을 등록하고, 작업을 마치면 DOCK OUT을 등록한다 |
| BR-YRD-VHC-006 | 도크 배정 | 차량이 입문하면 WCS가 아마노시스템에서 받은 입문 정보로 EWMS에 입문처리를 요청하고, 응답으로 DOCK 지정 결과를 받아 아마노시스템에 전송한다 (기존 'EWMS 처리, WCS는 결과 수신' → 입문처리 요청·응답으로 구체화) |
| BR-YRD-VHC-007 | 출고 차량정보 | 전부 EWMS에서 수신 |
| BR-YRD-VHC-009 | 차량 입문계획 | 차량 입문계획은 EWMS가 WCS로 push한다. 역수입 건은 FIS가 push한다 (협의 필요, Q-177) |
| BR-YRD-VHC-010 | 차량 입문계획 수신 원천 | 차량 입문계획은 EWMS(국내생산·국내OEM)와 FIS(역수입) 두 곳에서 받는다. 입고예정정보 수신(EWMS, MST-ORD BR-MST-ORD-016)과는 별개 인터페이스로 구분한다 (협의 필요, Q-177) |
| BR-YRD-VHC-011 | EWMS 차량 상태 통보 | WCS는 입문·DOCK IN·DOCK OUT·출문을 모두 EWMS에 통보한다. 입문은 입문처리 요청으로 보내고 응답으로 DOCK 지정 결과를 받는다. 출문은 아마노시스템에서 받은 정보를 통보한다 |
| BR-YRD-VHC-014 | DOCK IN/OUT 현황판 | DOCK IN·DOCK OUT 현황판은 WCS가 제공한다 |
| BR-YRD-VHC-016 | 차량 식별 키 | 입문 차량과 입고 오더·FO, 입문계획, DOCK 지정 결과는 차량번호를 키로 찾는다 |
| BR-YRD-VHC-020 | ATL DOCK의 DOCK IN·OUT | ATL 장비가 들어가는 DOCK은 센싱으로 DOCK IN·DOCK OUT을 체크할 수 있다 |
| BR-YRD-VHC-021 | DOCK 기준정보 | WCS도 도크관리 기준정보(DOCK 기준정보)를 관리한다. 사용자가 WCS에 등록하며, EWMS와 같은 데이터여야 한다 |
| BR-YRD-VHC-022 | DOCK별 이력 | 차량별 상태 이력이 아니라 DOCK별 이력이 필요하다 |
| BR-YRD-VHC-023 | 1층 DOCK 구성 | 1층 출고 DOCK은 22개다. ATL은 그중 2개 DOCK에 장착되어 팔레트 하역과 상차를 담당하며, ATL DOCK은 입고·출고를 같이 한다 |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-YRD-VHC-001 | WCS는 차량 상태를 입차 → DOCK IN → DOCK OUT → 출차 단계로 관리해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-002 | WCS는 FIS와 연동하여 역수입(해외생산·해외OEM) 입고 차량 입문계획을 수신해야 한다 (연동 개발). | WCS | 확정 |
| REQ-YRD-VHC-003 | WCS는 국내생산·국내OEM 입고 차량 입문계획을 EWMS에서 수신해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-005 | WCS는 게이트 관리 시스템(아마노시스템)과 연동하여 입차·출차 정보를 수신해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-006 | WCS는 차량이 도크에 붙으면 WCS PDA 또는 키오스크로 DOCK IN을, 작업 후 DOCK OUT을 등록받을 수 있어야 한다. | WCS | 확정 |
| REQ-YRD-VHC-007 | WCS는 아마노시스템에서 입문 정보를 받으면 EWMS에 입문처리를 요청하고, 응답으로 받은 DOCK 지정 결과를 아마노시스템에 전송해야 한다 (연동 개발). | WCS↔EWMS, WCS→아마노시스템 | 확정 |
| REQ-YRD-VHC-008 | WCS는 출고 차량(FO) 정보를 EWMS에서 수신해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-010 | WCS는 EWMS가 push하는 차량 입문계획과, 역수입 건에 대해 FIS가 push하는 차량 입문계획을 받아 입고 차량 입문 관리에 써야 한다. (협의 필요, Q-177) | EWMS→WCS, FIS→WCS | 확정 |
| REQ-YRD-VHC-011 | WCS는 차량의 입문(입문처리 요청)·DOCK IN·DOCK OUT·출문을 모두 EWMS에 통보해야 한다. | WCS→EWMS | 확정 |
| REQ-YRD-VHC-014 | WCS는 DOCK별 DOCK IN·DOCK OUT 현황판을 제공해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-015 | PDA·키오스크 등록·현황판 구성: ① PDA·키오스크 — 차량번호를 스캔·입력해 지정 DOCK과 차량 상태를 보여주고 DOCK IN·DOCK OUT을 등록한다 ② 현황판 — 지정 DOCK별 사용 중 차량, DOCK IN 시각·경과 시간, 입문 후 DOCK IN 대기 차량, DOCK OUT 후 미출문 차량, 기준 시간 초과 경고 | WCS | 확정 |
| REQ-YRD-VHC-016 | WCS는 차량번호를 키로 입문 차량을 입고 오더·FO, 입문계획, DOCK 지정 결과와 연결해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-020 | WCS는 ATL 장비가 들어가는 DOCK에서 센싱으로 들어온 DOCK IN·DOCK OUT 신호를 받아 등록할 수 있어야 한다. | 생기연 ECS(ATL)→WCS | 확정 |
| REQ-YRD-VHC-021 | DOCK IN·OUT 등록 수단 처리: ① 등록 수단(PDA / 키오스크 / ATL 센싱)을 등록 건에 기록한다 ② 같은 차량·DOCK에 이미 등록된 상태가 다른 수단으로 다시 들어오면 먼저 들어온 것을 유지하고 중복으로 기록만 한다 ③ ATL DOCK은 센싱을 기본으로 하되, 센싱이 없거나 실패하면 PDA·키오스크로 등록한다 ④ 센싱 신호에 차량번호가 없으면 해당 DOCK의 지정 차량(입문처리 응답)으로 연결한다. | WCS | 확정 |
| REQ-YRD-VHC-022 | WCS는 사용자가 DOCK 기준정보를 등록·수정하는 화면을 제공하고, 등록 데이터는 EWMS DOCK 데이터와 같게 유지해야 한다. | WCS | 확정 |
| REQ-YRD-VHC-023 | DOCK 기준정보 항목·정합: ① 항목 — 창고, DOCK 코드(EWMS 코드 그대로), DOCK명, 구분(1층 하역장 입고 DOCK / 출고 DOCK / 입출고 겸용(ATL DOCK)), 층, 위치(MST-LOC 로케이션 연결), ATL 설치 여부(센싱 대상), 사용 여부, 등록·수정 이력 ② 정합 — 입문처리 응답의 지정 DOCK이나 ATL 센싱 DOCK이 WCS DOCK 기준정보에 없으면 처리는 계속하되 경고하고 미등록 DOCK 목록에 올려 사용자가 등록하게 한다 ③ 출고 DOCK 코드는 피킹 W/T·출고완료 확정(OUB-CFM)의 출고 DOCK과 같은 코드를 쓴다 ④ 현황판은 DOCK 기준정보의 사용 DOCK 전체를 보여주고, 차량이 없는 DOCK은 비어 있음으로 표시한다. | WCS | 확정 |
| REQ-YRD-VHC-024 | WCS는 DOCK별 이력(어느 차량이 언제 DOCK IN·DOCK OUT 했는지)을 남기고 조회할 수 있어야 한다. | WCS | 확정 |
| REQ-YRD-VHC-025 | DOCK별 이력 설계: ① 이력 단위 — DOCK 코드별로 DOCK 지정(입문처리 응답)·DOCK IN·DOCK OUT 이벤트를 한 행씩 쌓는다(차량번호, 입고 오더 / FO, 입고·출고 구분, 시각, 등록 수단 PDA / 키오스크 / ATL 센싱, 등록자) ② 점유 구간 — DOCK IN~DOCK OUT을 한 건의 점유로 묶어 점유 시간을 계산한다 ③ 조회 — PC 화면에서 기간·DOCK·구분으로 조회하고 엑셀로 내려받으며, 일·DOCK별 처리 대수·평균 점유 시간·가동률을 집계한다 ④ DOCK IN 없이 DOCK OUT이 들어오면 누락으로 표시한다. DOCK OUT 없이 다음 DOCK IN이 들어오면 거부하지 않고 받으며, 앞의 점유를 누락(DOCK OUT 없음)으로 표시하고 닫는다(거부하면 그 DOCK을 쓸 수 없음) ⑤ 보관 기간은 창고 기초속성(MST-OPT)으로 둔다(값 Q-182). | WCS | 확정 |
| REQ-YRD-VHC-026 | WCS DOCK 기준정보에 1층 출고 DOCK 22개를 등록하고, 그중 ATL이 장착된 2개 DOCK을 ATL 설치 여부(센싱 대상)로 표시하고 구분을 입출고 겸용으로 둬야 한다(REQ-YRD-VHC-023 ①). | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| FIS→WCS | 역수입 건 차량 입문계획 생성 시 (push) — 협의 필요 Q-177 | 차량번호, 입고 오더, 도착 예정 시각 (항목 협의 필요, 해외생산·해외OEM) |
| EWMS→WCS | 차량 입문계획 생성 시 (push, 국내생산·국내OEM) — 협의 필요 Q-177 | 차량번호, 입고 오더, 도착 예정 시각 (항목 협의 필요) |
| EWMS→WCS | 출고 차량(FO) 정보 생성 시 | FO, 차량번호, 출고 오더 (항목 협의 필요) |
| 아마노시스템→WCS | 입문(입차)·출문(출차) 시 | 차량번호, 구분(입문/출문), 시각 |
| WCS→EWMS | 입문 정보 수신 시 | 입문처리 요청: 차량번호, 입고 오더 / FO, 입문 시각 (항목 Q-135) |
| EWMS→WCS | 입문처리 요청 응답 | DOCK 지정 결과: 차량번호, 지정 DOCK |
| WCS→아마노시스템 | 지정 DOCK 수신 시 | 차량번호, 지정 DOCK |
| WCS PDA·키오스크→WCS | DOCK IN·DOCK OUT 등록 시 | 차량번호, 도크, 상태, 등록 시각, 등록자 |
| 생기연 ECS(ATL)→WCS | ATL DOCK 센싱 시 | DOCK, 차량번호(있으면), DOCK IN/OUT 구분, 시각 (항목 Q-180) |
| WCS→EWMS | DOCK IN·DOCK OUT 등록 시 | DOCK IN 통보 / DOCK OUT 통보: 차량번호, 도크, 시각 |
| WCS→EWMS | 출문 정보 수신 시 | 출문 통보: 차량번호, 출문 시각 |

## 7. 예외 처리
- 미정

## 8. 미결 사항
- Q-182: DOCK별 이력 보관 기간 값 (DOCK별 이력 설계 REQ-YRD-VHC-025 채택)
- Q-181: EWMS와 같은 DOCK 데이터를 보장하는 방법(사용자 등록 후 대사 / EWMS 코드 목록 제공) (사용자 등록·EWMS와 같은 데이터·REQ-YRD-VHC-023 채택 결정)
- Q-180: DOCK IN·OUT 등록 수단 상세 — 키오스크 제공 주체·설치 위치, ATL 센싱 신호 송신 주체(생기연 ECS 가정)·항목·대상 DOCK (REQ-YRD-VHC-015·021 채택, PDA 또는 키오스크 등록, ATL DOCK 센싱 체크 가능 결정)
- Q-177: [추가 협의] 차량 입문계획 수신 — 원천 구분(국내생산·국내OEM = EWMS, 역수입 = FIS), push 방식, 항목·시점 (EWMS·FIS 이원 수신은 차량 입문계획만)
- Q-062: [추후 정리] 게이트 관리 시스템(아마노시스템) 연동 방식·항목, 사전 차량정보 없는 차량 입차 시 처리
- Q-135: [추후 정리] 차량정보 수신 인터페이스 항목·시점 — EWMS(차량 입문계획 push, 출고 FO·도크 배정), FIS(역수입 건 차량 입문계획 push)

---
[YRD-VHC 끝 · 다음 입력: 없음 — 마지막 키워드]
