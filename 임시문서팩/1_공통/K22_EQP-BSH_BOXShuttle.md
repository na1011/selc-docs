<!-- AI 입력팩 · K22/40 · EQP-BSH · 묶음 E · 기준일 2026-11-02 -->
# [K22/40] [EQP-BSH] BOX Shuttle

| 항목 | 내용 |
|---|---|
| 대분류 | 공통 — 시스템·공통·기준정보 (폴더 1_공통) |
| 입력 순서 | 22 / 40 (묶음 E: 보관설비) |
| 선행 키워드 | K04 SYS-ERR, K08 MST-OPT, K10 MST-ALC, K12 MST-EQP, K17 INV-UNT, K18 INV-WT, K20 EQP-VND, K21 EQP-ASRS |
| 후행 키워드 | K24 EQP-MIX, K27 INB-DVN, K31 OUB-GNT, K32 OUB-MIX, K38 INV-CNT |
| 상태 | 일부확정 · BR 17 · REQ 확정 11 / 확인필요 2 · 미결 2 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K04 SYS-ERR 통합 에러관리**: 설비사·설비마다 다른 에러코드를 WCS가 취합해 통합 에러코드로 관리한다. 에러마다 작업자 전달 메시지, 처리방법 메시지, 후행 API를 연결해 두고, 에러가 들어오면 연결된 메시지를 작업자에게 전달하고 후행 API로 후속 처리(Reject 등록, 재고 오류 표시, 재할당, Task 보류 등)를 잇는다.
- **K08 MST-OPT 창고 기초속성**: 창고별로 제어해야 하는 옵션을 변수로 담아 창고 기초속성으로 관리한다. 각 프로세스는 필요한 시점에 해당 창고의 기초속성 변수를 읽어 동작을 제어한다.
- **K10 MST-ALC 할당 전략**: 프로세스별로 어떤 재고를 작업 대상으로 쓸지 정하는 규칙을 '할당 전략'이라는 설정 관리로 통합한다. 출고 할당(FULL 팔레트·GANTRY·3층 출고 구성·GTP·직송 이적), 사전 보충·긴급 보충, 재할당, 재고실사 배출, 공팔레트 묶음 피킹 등 재고를 쓰는 모든 프로세스는 각자 규칙을 코드로 갖지 않고 할당 전략 설정을 참조해 후보 재고를 거르고 순서를 정한다.
- **K12 MST-EQP 설비 기준정보 · 팔레트 유형**: WCS는 자동화 설비를 기준정보(설비 기준정보)로 관리한다. 설비마다 장비 특성(작업 단위, 처리 능력, 적재 제약, 투입·배출 포트, 사용 팔레트 유형 등)이 다르므로, 공통 속성과 설비 유형별 속성을 정의하여 작업지시 대상 ECS 결정, 설비 배정, 설비 접점 처리, 공팔레트 공급의 기준으로 사용한다.
- **K17 INV-UNT 재고 관리단위(invn_id) · 위치 관리**: WMS 기본 사상에 따라 물리적 위치(존·로케이션·셀)와 논리적 관리단위(UOM 관리단위)를 분리한다. 논리적 관리단위는 invn_id로 식별하며 BOX일 수도, 팔레트일 수도 있다.
- **K18 INV-WT W/T 상태 · 가용재고**: EWMS가 W/T를 생성·확정하고 WCS는 이를 연동하여 재고 상태를 관리한다. W/T 생성 시 WCS 재고로 잡히지만 가용재고가 아니며, W/T 확정 후 가용재고가 되어 출고지시가 가능하다.
- **K20 EQP-VND 설비사 맵핑 · 설비 접점**: 자동화 설비별 설비사를 정의하고, 설비사가 다른 설비 사이에서 BOX/팔레트가 인계되는 설비 접점의 발생 위치를 정리한다. 설비사별로 ECS가 따로 있으므로 설비 접점에서는 설비끼리 직접 연동하지 않고 WCS가 중간 처리한다.
- **K21 EQP-ASRS AS/RS**: 팔레트 단위 핵심 자동화 보관 설비로, 일반/디베닝입고 완료 팔레트와 공팔레트 묶음을 보관한다. 1·3·4층에 입고대·배출구가 있으며, 1층에서 FULL 팔레트 출고를 배출하는 곳을 출고대(1층 출고대)라 한다.

## 머리말
> 분류: 업무=보관 / 설비=BOX Shuttle
> 관련 시스템: WCS, ECS, EWMS | 관련 키워드: INB-DVN, EQP-ASRS, EQP-MIX, EQP-VND, INV-WT, OUB-GNT, OUB-MIX, SYS-ERR
> 상태: 일부확정

## 1. 개요
3층의 중소형 제품 대상 BOX 단위 보관 설비. 팔레트화 없이 BOX 그대로 입고·보관·피킹한다. 입고 경로는 A(디베닝 직접 투입)와 B(3층 출고 구성 후 잔량 보관) 두 가지이며, 3층 사전 보충 BOX도 디팔레타이징 후 같은 방식으로 투입한다. 3층 디팔레타이징·MIXED 팔레타이징 설비는 별도 키워드(EQP-MIX)로 분리하며, 같은 현대무벡스 설비라 두 설비 간 BOX 인계는 내부 인계로 WCS가 작업을 지시한다. 설비사는 현대무벡스이며, 로지스올 설비와 만나는 설비 접점은 경로 A 투입(P-02)이다. 3층 출고 구성 BOX는 3층 MIXED 팔레타이징로봇(현대무벡스)으로 보내는 내부 인계다 (경로 B 투입 P-04·팔레타이징로봇 이송 P-05는 폐기, EQP-VND).

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): BOX Shuttle, 디베닝, 디팔레타이징, MIXED 팔레타이징, 잔량, 적치셀, W/T 확정, AS/RS, EWMS, WCS, ECS, GANTRY, BCR, 팔레타이징로봇, W/T, 제조번호, 하역 로봇, 설비 접점, BOX 식별자, 설비사, 현대무벡스, 로지스올, 3층 MIXED 팔레타이징로봇, 제품코드, 재고 관리단위, 개체화, 재고 이동 로그, 창고 기초속성, 팔레타이징, 3층 디팔레타이징 범위, 대시보드, 파워유저, 체적정보, DeWrapping, 가용재고, 도착예정 수량, 사전 보충, 출고예정, 긴급 보충, 입고월, 적치 W/T, 피킹 W/T, 디베닝 하역 대상, LOT, 자동화구역, W/T 생성, HU 생성, RCV9, 통합 에러관리, 후행 API, 전략 이력

## 3. 프로세스 흐름
입고 경로 A (디베닝 직접 투입, AS/RS 미경유):
0. (WCS→BOX Shuttle ECS) 디베닝 하역 대상 수신 시 EWMS 지정 BOX Shuttle 적치 수량으로 투입 예정(입고번호·제품·LOT·수량)을 사전 전달 (BR-EQP-BSH-015)
1. 1층 하역 로봇 하역 → 3층 이동 → BCR 스캔 → 속성값 = BOX Shuttle
2. (WCS→BOX Shuttle ECS) 투입 지시 (BOX 식별자 전달, 적치 위치 미지정) → BOX Shuttle 자동 할당 적치
3. BOX Shuttle → WCS 적치 통보 → (WCS→EWMS) BOX 단위 적치 W/T 생성 요청 → 확정 (BR-EQP-BSH-016)

입고 경로 B (3층 출고 구성 후 잔량):
1. BOX Shuttle 인출(AS/RS 팔레트 해체 BOX 포함) → 3층 MIXED 팔레타이징 (EQP-MIX)
2. (현대무벡스 ECS(3층 MIXED 팔레타이징)→WCS) 잔량 BOX 정보 통보
3. (WCS→BOX Shuttle ECS) 잔량 BOX 투입 지시 — 현대무벡스 내부 인계, WCS 작업지시
4. BOX Shuttle 자동 할당 적치 → WCS 통보 (W/T 필요 여부 Q-016)

3층 사전 보충 (OUB-MIX BR-OUB-MIX-013, 방식 REQ-OUB-MIX-014):
1. 출고예정 기반 AS/RS 팔레트 3층 배출 → 랩핑 제거 → 3층 디팔레타이징
2. (WCS→BOX Shuttle ECS) 보충 BOX 투입 지시 → BOX Shuttle 자동 할당 적치 → WCS 통보 (자동화구역 안 이동이라 W/T 없음, OUB-MIX REQ-OUB-MIX-011 ⑤)

AS/RS 팔레트 3층 해체 BOX (할당 우선순위 2순위, OUB-MIX BR-OUB-MIX-021):
1. AS/RS 팔레트 3층 배출 → 랩핑 제거 → 3층 디팔레타이징 → 해체 BOX 전량 BOX Shuttle 투입 지시 → 자동 할당 적치 → WCS 통보
2. 적치 완료 후 피킹 W/T에 할당 → BOX Shuttle 인출 → 3층 MIXED 팔레타이징 (MIXED 직투입 불가)

3층 긴급 보충 (OUB-MIX BR-OUB-MIX-015·017):
1. 부족 감지 → AS/RS 팔레트 3층 배출 → 랩핑 제거 → 3층 디팔레타이징 → 해체 BOX 전량 BOX Shuttle 적치 (MIXED 직투입 안 함)
2. 적치 완료 후 대상 W/T에 할당 → BOX Shuttle 인출 → 3층 MIXED 팔레타이징

출고:
1. WCS 출고 작업지시
2. BOX 인출 → WCS 통보
3. 출고 구성 위치(3층 MIXED 팔레타이징로봇)로 이송 — 현대무벡스 내부 인계
4. 팔레트 출고 완료 결과 → WCS 통보

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-EQP-BSH-001 | 디베닝 속성값 = BOX Shuttle | 경로 A, AS/RS 미경유 |
| BR-EQP-BSH-002 | 3층 MIXED 팔레타이징 후 잔량 발생 | 경로 B, BOX Shuttle 보관 |
| BR-EQP-BSH-003 | BOX Shuttle 적치 위치 결정 | WCS는 적치 위치를 지정하지 않음, BOX Shuttle이 자동 할당 (경로 A·B 공통) |
| BR-EQP-BSH-005 | BOX Shuttle ↔ 3층 MIXED 팔레타이징 간 BOX 인계 | 설비 접점으로 WCS가 중간 처리 (BR-EQP-MIX-002) |
| BR-EQP-BSH-006 | BOX 식별자 | 제조번호관리 상품 = 제조번호(S/N), 그 외 = 제품코드 |
| BR-EQP-BSH-007 | 3층 출고 구성 재고 사용 순서 | 피킹 W/T 할당 시에는 할당 우선순위로 쓴다: 1순위 BOX Shuttle 재고, 2순위 AS/RS 팔레트 3층 배출·디팔레타이징. 출고 구성·부족 처리는 OUB-MIX. 중소형 BOX 낱개 오더용 출고예정 기반 사전 보충은 한다(OUB-MIX BR-OUB-MIX-013) |
| BR-EQP-BSH-008 | BOX 재고 관리단위 | 개체화된 BOX의 invn_id는 위치한 적치셀의 위치 코드다. 셀 간 이동 완료 시 재고 이동 로그(from·to 한 건)를 쌓고 그 처리 결과로 invn_id가 목적 셀 코드로 반영된다(직접 수정 없음) (INV-UNT BR-INV-UNT-007·008·011) |
| BR-EQP-BSH-009 | BOX Shuttle 용량 부족 | 3층 설비가 알아서 멈춘다. WCS가 작업을 멈추는 처리는 하지 않는다 (같은 날 WCS 멈춤 → 설비 자체 멈춤으로 정정) |
| BR-EQP-BSH-010 | 용량 임계치 알림 | 적치율이 80% 임계치에 도달하면 알림을 준다. 기준값은 창고 기초속성 값으로 저장해 관리한다 |
| BR-EQP-BSH-011 | 공간 확보 방안 | BOX Shuttle 재고를 3층 팔레타이징 작업지시로 팔레트화한 뒤 AS/RS로 이동 처리해 공간을 확보한다 — 협의 필요 (Q-131) |
| BR-EQP-BSH-012 | 3층 사전 보충 BOX | 중소형 BOX 낱개 오더 사전 보충(OUB-MIX BR-OUB-MIX-013)으로 디팔레타이징한 BOX를 BOX Shuttle에 적치해 피킹 W/T 할당 시 1순위 재고로 쓴다 (경로·방식 OUB-MIX REQ-OUB-MIX-014). 긴급 보충 BOX도 MIXED에 바로 넣지 않고 BOX Shuttle에 적치한 뒤 쓴다(OUB-MIX BR-OUB-MIX-017). 보충 중 적치율 임계치에 도달하면 보충을 막지 않고 사용자 알람으로 사전 조치한다(OUB-MIX BR-OUB-MIX-018) |
| BR-EQP-BSH-013 | 3층 디팔레타이징 BOX 경유 | AS/RS 팔레트를 3층에서 해체한 BOX(할당 우선순위 2순위·사전 보충·긴급 보충, 전량·부분 해체 공통)는 MIXED에 바로 투입할 수 없어 모두 BOX Shuttle에 적치한 뒤 인출해 MIXED에 투입한다 (OUB-MIX BR-OUB-MIX-021) |
| BR-EQP-BSH-014 | BOX 사용 순서 | GANTRY와 같이 BOX Shuttle 셀 위치를 셀 마스터로 관리하고 셀별 피킹 우선순위 순서로 BOX를 처리한다(WCS 기준). 다른 BOX 단위 설비에도 같은 방식을 쓴다 (MST-LOC BR-MST-LOC-030). 입고월 FIFO를 우선하고, 같은 입고월 안에서 셀 피킹 우선순위 순으로 쓴다. |
| BR-EQP-BSH-015 | 경로 A 인계 방식(P-02) | 사전 지시로 한다. EWMS가 디베닝 하역 대상에 BOX Shuttle 적치 수량을 지정하면, WCS가 전체 수량에서 BOX Shuttle 수량과 나머지 경로 수량을 미리 계산해 ECS에 전달한다 (INB-DVN BR-INB-DVN-029·030) |
| BR-EQP-BSH-016 | 경로 A 적치 W/T 단위 | 디베닝 경로 A로 BOX Shuttle에 적치한 BOX는 BOX 단위로 W/T를 처리한다. BOX마다 적치 완료 시 적치 W/T 생성을 요청하고 확정한다 |
| BR-EQP-BSH-017 | BOX Shuttle 예외 | 투입 거부·적치 실패·인출 실패 등 BOX Shuttle 예외는 설비 에러코드로 받아 통합 에러관리(SYS-ERR)로 처리한다. 에러별 작업자 전달 메시지·처리방법 메시지·후행 API는 설비 에러코드 일괄 정리 때 정한다(SYS-ERR Q-211, 기본 연결 추천 REQ-SYS-ERR-006) |
| BR-EQP-BSH-018 | 공간 확보 운영 기준 | 기본은 운영 관리로 한다. 적치율이 임계치에 도달하면 ① 사전 보충 소요량에 BOX Shuttle 여유 용량 상한을 두고 ② 3층 출고 할당 때 BOX Shuttle 재고를 우선 소진한다(할당 우선순위 1순위 유지). 팔레트화 후 AS/RS 이동(BR-EQP-BSH-011)은 설비 경로가 확인되면 추가한다 |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-EQP-BSH-001 | WCS는 경로 A/B 각각에 대해 BOX Shuttle 입고 지시를 내려야 한다. | WCS | 확인필요 |
| REQ-EQP-BSH-002 | BOX Shuttle은 적치 완료를 WCS에 통보하고 WCS는 EWMS에 전달해 적치 W/T를 확정해야 한다. 경로 A(디베닝)는 BOX 단위로 적치 완료 시 W/T 생성 요청·확정한다(BR-EQP-BSH-016). | ECS, WCS | 확정 |
| REQ-EQP-BSH-003 | WCS는 BOX 단위 출고 작업지시 및 인출·출고완료 결과를 수신해야 한다. | WCS | 확인필요 |
| REQ-EQP-BSH-005 | WCS는 BOX Shuttle 투입 지시 시 적치 위치를 지정하지 않으며, BOX Shuttle이 자동 할당한 적치셀을 적치 완료 통보로 수신해 관리해야 한다. | WCS | 확정 |
| REQ-EQP-BSH-006 | WCS는 BOX 식별자를 제조번호관리 상품은 제조번호(S/N), 그 외 상품은 제품코드로 BOX Shuttle과 주고받아야 한다. | WCS | 확정 |
| REQ-EQP-BSH-007 | BOX Shuttle 용량이 부족하면 3층 설비가 자체적으로 멈추며, WCS는 별도로 작업을 멈추지 않는다. WCS는 적치율이 임계치(창고 기초속성 값, 기본 80%)에 도달하면 알림을 제공해야 한다. | WCS, ECS | 확정 |
| REQ-EQP-BSH-008 | 적치율은 사용 셀 수 ÷ 전체 사용 가능 셀 수로 계산한다. 알림은 대시보드와 파워유저에게 보내고, 임계치 도달·해소 시점을 이력으로 남긴다. 설비가 용량 부족으로 멈춘 상태는 설비 운영 상태 수신(MST-EQP REQ-MST-EQP-009)으로 반영해 대시보드에 표시한다. (기준값 = 창고 기초속성은 확정, BR-EQP-BSH-010) | WCS | 확정 |
| REQ-EQP-BSH-009 | WCS는 3층 사전 보충 BOX(AS/RS 팔레트 → 3층 디팔레타이징 → BOX Shuttle)에 대해 BOX Shuttle 투입 지시를 내리고, 적치 완료 전까지 도착예정 수량으로 관리해야 한다(가용재고 아님, 자동화구역 안 이동이라 W/T 없음 — OUB-MIX REQ-OUB-MIX-011 ⑤). | WCS | 확정 |
| REQ-EQP-BSH-010 | WCS는 3층에서 해체한 BOX 전량에 대해 BOX Shuttle 투입을 지시하고, 적치 완료 후 피킹 W/T에 할당해 MIXED 팔레타이징로봇으로 인출을 지시해야 한다(OUB-MIX REQ-OUB-MIX-023). | WCS | 확정 |
| REQ-EQP-BSH-011 | WCS는 BOX Shuttle 셀별 피킹 우선순위를 셀 마스터로 관리하고, BOX Shuttle 재고를 출고 구성에 할당할 때 입고월 FIFO를 우선하고 같은 입고월 안에서 피킹 우선순위 순서로 인출 대상을 정해야 한다(적치 위치는 BOX Shuttle이 자동 결정, BR-EQP-BSH-003). | WCS | 확정 |
| REQ-EQP-BSH-012 | WCS는 경로 A BOX Shuttle 투입 예정(입고번호, 제품, LOT, 수량)을 디베닝 하역 대상 수신 시 BOX Shuttle ECS에 미리 전달하고, BOX Shuttle의 적치 완료 통보(BOX 식별자·적치셀)를 투입 예정과 연결해 3층 BCR → BOX Shuttle 셀로 재고를 옮겨야 한다(BR-EQP-BSH-015). | WCS | 확정 |
| REQ-EQP-BSH-013 | 경로 A BOX 단위 적치 W/T: ① BOX Shuttle 적치 완료 통보(BOX 식별자·적치셀)를 받을 때마다 해당 BOX 1건으로 EWMS에 적치 W/T 생성을 요청한다(BOX 식별자, 제품, LOT, 수량, DEST 9015·RCV9 — 디베닝 팔레타이징과 같은 DEST, INB-DVN BR-INB-DVN-021) ② 생성 응답을 받으면 바로 확정하고, 확정 시 해당 BOX를 가용재고로 바꾼다 ③ HU 생성·PACK은 없다(INB-DVN REQ-INB-DVN-017 ⑤) ④ EWMS 단절 시 요청을 대기열에 쌓아 복구 후 순서대로 재전송한다. | WCS | 확정 |
| REQ-EQP-BSH-014 | WCS는 BOX Shuttle 적치율이 임계치(창고 기초속성) 이상이면 3층 사전 보충 소요량을 여유 용량(임계치까지 남은 수용량) 안으로 줄이고, 긴급 보충은 줄이지 않아야 한다. 줄어든 사전 보충분은 이후 긴급 보충으로 채워진다. 상한 적용·해제 시점과 줄인 수량을 전략 이력으로 남긴다(BR-EQP-BSH-018). | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| WCS→ECS(BOX Shuttle) | 디베닝 하역 대상 수신 시 (경로 A 사전 지시) | 입고번호, 제품, LOT, BOX Shuttle 투입 예정 수량 (BR-EQP-BSH-015) |
| WCS→ECS | 투입 시 | BOX 식별자(제조번호 또는 제품코드), 수량 (적치 위치 미지정) |
| ECS→WCS | 적치 완료 시 | BOX 식별자, 자동 할당 적치셀 |
| WCS→EWMS | 경로 A BOX 적치 완료 시 (BOX마다) | 적치 W/T 생성 요청·확정: BOX 식별자, 제품, LOT, 수량, DEST 9015·RCV9 (BR-EQP-BSH-016) |
| WCS→ECS | 출고 지시 시 | BOX 식별자, 목적지(3층 MIXED 팔레타이징로봇) |
| ECS→WCS | 인출/출고 완료 시 | BOX 식별자, 결과 |

## 7. 예외 처리
- 용량 부족: 3층 설비가 자체적으로 멈춤(WCS 처리 없음), WCS는 80% 임계치 도달 시 알림 (REQ-EQP-BSH-007), 공간 확보 방안은 협의 필요 (Q-131)
- 그 외 예외는 통합 에러관리(SYS-ERR)의 에러코드별 처리로 정리한다 (BOX Shuttle 기본 연결 REQ-SYS-ERR-006)

## 8. 미결 사항
- Q-016: [추가 협의] 경로 B(3층 MIXED 잔량 → BOX Shuttle) W/T 필요 여부 — 자동화구역 안 이동이라 W/T 없음으로 볼지
- Q-131: [추가 협의] 팔레트화 공간 확보(BR-EQP-BSH-011) 설비 경로 — BOX Shuttle(현대무벡스) → 3층 입고 팔레타이징로봇(로지스올) 접점 재정의(P-05 폐기), 대상 BOX 기준(입고월 오래됨·출고예정 없음)·사용자 지시 시점 (기본은 사전 보충 상한·BOX Shuttle 우선 소진 운영 관리 결정)

---
[EQP-BSH 끝 · 다음 입력: K23 EQP-GNT GANTRY]
