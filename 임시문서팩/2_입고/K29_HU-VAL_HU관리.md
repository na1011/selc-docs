<!-- AI 입력팩 · K29/40 · HU-VAL · 묶음 F · 기준일 2026-11-02 -->
# [K29/40] [HU-VAL] 공팔레트 · HU 확인/생성

| 항목 | 내용 |
|---|---|
| 대분류 | 입고 (폴더 2_입고) |
| 입력 순서 | 29 / 40 (묶음 F: 입고·HU) |
| 선행 키워드 | K10 MST-ALC, K12 MST-EQP, K13 MST-PRD, K15 MST-SN, K17 INV-UNT, K18 INV-WT, K21 EQP-ASRS, K26 INB-PLT, K27 INB-DVN |
| 후행 키워드 | K33 OUB-MAN, K34 OUB-TRF |
| 상태 | 일부확정 · BR 20 · REQ 확정 19 / 확인필요 3 · 미결 2 |

> 읽는 규칙은 C01 공통 컨텍스트 §1을 따른다. 용어 정의는 C02 GLOSSARY를 따른다(이 문서에는 §2 용어를 넣지 않음). 확정만 설계 기준, 확인필요·미결(Q)은 가정으로 표시.

## 0. 선행 맥락 (앞 순서 관련 키워드 요약)
- **K10 MST-ALC 할당 전략**: 프로세스별로 어떤 재고를 작업 대상으로 쓸지 정하는 규칙을 '할당 전략'이라는 설정 관리로 통합한다. 출고 할당(FULL 팔레트·GANTRY·3층 출고 구성·GTP·직송 이적), 사전 보충·긴급 보충, 재할당, 재고실사 배출, 공팔레트 묶음 피킹 등 재고를 쓰는 모든 프로세스는 각자 규칙을 코드로 갖지 않고 할당 전략 설정을 참조해 후보 재고를 거르고 순서를 정한다.
- **K12 MST-EQP 설비 기준정보 · 팔레트 유형**: WCS는 자동화 설비를 기준정보(설비 기준정보)로 관리한다. 설비마다 장비 특성(작업 단위, 처리 능력, 적재 제약, 투입·배출 포트, 사용 팔레트 유형 등)이 다르므로, 공통 속성과 설비 유형별 속성을 정의하여 작업지시 대상 ECS 결정, 설비 배정, 설비 접점 처리, 공팔레트 공급의 기준으로 사용한다.
- **K13 MST-PRD 제품마스터**: 입고 경로, 보관설비, 출고설비를 결정하는 제품마스터 속성을 정의한다. Product Code는 제품마스터의 하위 속성이며, 이를 이용한 제조번호 정합성 체크도 여기서 다룬다.
- **K15 MST-SN 제조번호(S/N) 관리**: 평택 CDC는 LOT 생성 전략에서 제조번호를 LOT 속성으로 선택하지 않는다(물류 작업지시 효율 저하 우려, 후보 속성으로는 존재, MST-LOT). 대신 시리얼 마스터를 두어, 시리얼번호가 입고되어 최초 생성될 때, 설비로부터 시리얼번호 단위 결과 실적을 받을 때, 출고하면서 시리얼번호 단위 검수 결과를 받을 때 이력 로그를 쌓아 추적한다.
- **K17 INV-UNT 재고 관리단위(invn_id) · 위치 관리**: WMS 기본 사상에 따라 물리적 위치(존·로케이션·셀)와 논리적 관리단위(UOM 관리단위)를 분리한다. 논리적 관리단위는 invn_id로 식별하며 BOX일 수도, 팔레트일 수도 있다.
- **K18 INV-WT W/T 상태 · 가용재고**: EWMS가 W/T를 생성·확정하고 WCS는 이를 연동하여 재고 상태를 관리한다. W/T 생성 시 WCS 재고로 잡히지만 가용재고가 아니며, W/T 확정 후 가용재고가 되어 출고지시가 가능하다.
- **K21 EQP-ASRS AS/RS**: 팔레트 단위 핵심 자동화 보관 설비로, 일반/디베닝입고 완료 팔레트와 공팔레트 묶음을 보관한다. 1·3·4층에 입고대·배출구가 있으며, 1층에서 FULL 팔레트 출고를 배출하는 곳을 출고대(1층 출고대)라 한다.
- **K26 INB-PLT 팔레트 입고 (일반입고 / 대형)**: 1층에서 하역 또는 입고 수작업 영역에서 수작업 팔레타이징한 팔레트를 1층 AS/RS에 투입한다. 투입 시점에 WCS가 EWMS 내역을 확인하여 AS/RS 보관을 결정하며, AS/RS 용량이 부족하면 3층 출고대로 배출한다(용량 부족 배출, Bypass와 별개). 1층 입고대 Reject는 Bypass와 구분하며, ECS가 판단해 에러 코드를 포함해 3층 출고대로 배출한다.
- **K27 INB-DVN 디베닝 입고**: 디베닝 대상 중소형 BOX 제품을 1층에서 로봇으로 하역해 3층으로 이동하고, BCR 스캔 후 입고정보 속성값에 따라 팔레타이징(→AS/RS) 또는 BOX Shuttle로 분기한다. EWMS 재고위치는 EWMS 디베닝 작업지시 시 1층 입고장(9011·RCV1)에서 디베닝설비 입고장(Stock Type 9015·BIN RCV9)으로 전환되며, WCS도 이 시점 위치셀 RCV9에 자동화 설비 재고로 잡는다.

## 머리말
> 분류: 업무=HU / 설비=AS/RS(공팔레트 보관), 팔레타이징로봇
> 관련 시스템: EWMS, WCS, ECS | 관련 키워드: INB-PLT, INB-DVN, INV-WT, OUB-MAN, MST-EQP, MST-SN, MST-PRD, INV-UNT
> 상태: 일부확정

## 1. 개요
팔레트 ID의 WCS 사용 가능 여부를 판단하기 위해 EWMS HU를 확인·생성한다. 공팔레트는 묶음 단위로 AS/RS에 재고 보관하여 관리한다. 팔레타이징 유형별로 HU 확인/생성 주체를 정의한다. WCS는 자동화 설비 재고에 대해 HU PACK·HU UNPACK·HU SWAP 기능을 제공하며, 작업자가 WCS PDA에서 특정 셀의 재고현황을 조회해 처리한다. 작업 가능한 셀은 작업자가 접근할 수 있는 셀로 한정하며, 셀 기준정보 설정을 기준으로 한다. EWMS API 호출이 실패하면 작업을 처리하지 못하게 막는다. HU 검증과 작업 반영은 EWMS가 제공하는 API(HU 조회(HU 스캔 시), HU 생성, HU 삭제, PACK, UNPACK, SWAP)로 하며, WCS PDA 처리 시 WCS가 호출한다. HU는 팔레트 ID이므로 팔레트 재고는 invn_id가 HU와 같다.

## 2. 용어
이 문서에서 쓰는 용어(정의는 C02 GLOSSARY): HU, HU 확인, HU 생성, 공팔레트, 팔레트 ID, W/T 확정, 매거진, 일반입고, 디베닝입고, 수작업 팔레타이징, 1층 자동화 팔레타이징, MIXED 팔레타이징, EWMS, WCS, ECS, AS/RS, 팔레타이징로봇, 팔레타이징, 디팔레타이징, 잔량, W/T, 팔레트 유형, 공팔레트 묶음, Batch, 재고 관리단위, 현대무벡스, 시리얼 마스터, 변경 TASK번호, Task, GTP, EWMS PDA, WCS PDA, 예약수량, 작업 잠금, HU 삭제, HU PACK, HU UNPACK, HU SWAP, LOT, 시리얼 이력 로그, 파워유저, 팔레트갈이, 디베닝, 수작업 영역, 재고대사, 제조번호, Workflow, 입고대, GANTRY, 피킹 W/T, 적치 W/T, Reject

## 3. 프로세스 흐름
EWMS 팔레타이징(검증) 순서:
1. HU 확인: 사용하려는 HU가 EWMS에 채번되어 있는지 확인
2. HU 생성: 없으면 신규 생성
3. W/T HU 매핑/확정: EWMS에서 HU와 W/T 매핑

WCS 팔레타이징 결과 연동: EWMS 처리 순서대로 연동 (실시간 또는 배치)

HU PACK·UNPACK·SWAP (WCS PDA):
1. (작업자, WCS PDA) 접근 가능한 셀 스캔 → (WCS) 셀 재고현황 조회 (invn_id, HU, 제품, LOT, 수량, 상태)
2. (작업자) 작업 선택(PACK / UNPACK / SWAP)과 대상 재고·수량 선택
3. (작업자) HU 스캔 → (WCS→EWMS) HU 조회 API로 검증, 새 HU면 HU 생성 API
4. (WCS→EWMS) PACK / UNPACK / SWAP API 호출
5. (WCS) 결과를 재고 이력으로 반영 (팔레트 invn_id = HU, 같은 변경 TASK번호)

## 4. 비즈니스 규칙
| ID | 조건 | 처리 |
| --- | --- | --- |
| BR-HU-VAL-001 | W/T 확정 | 반드시 HU 생성 후 진행 |
| BR-HU-VAL-002 | WCS 팔레타이징 결과 연동 | EWMS 처리 순서(HU 확인 → 생성 → W/T 매핑)대로, 배치 가능 |
| BR-HU-VAL-004 | 디팔레타이징 후 매거진 취합 배출 건 | 최하단 대표 팔레트 ID + 적재 팔레트 ID를 AS/RS ECS가 취합하여 WCS로 전달, 일반입고와 동일하게 검증 |
| BR-HU-VAL-006 | 디베닝입고 / 1층 자동화 팔레타이징 / MIXED 팔레타이징 | 공팔레트 보관(또는 AS/RS 입고) 전 HU 확인 선행 → WCS HU 확인 불필요 |
| BR-HU-VAL-007 | 분할 팔레트 잔량 | 다음 일련번호 팔레트 ID로 HU 확인 (OUB-MAN) |
| BR-HU-VAL-008 | 디팔레타이징 후 공팔레트 | 디팔레타이징 설비 바로 옆 매거진에 팔레트 유형별로 보관한다. 1층 매거진 만재 묶음은 1층 AS/RS 입고대로 옮겨 보관한다(이동 구간 Workflow, MST-EQP BR-MST-EQP-024) (MST-EQP) |
| BR-HU-VAL-009 | 공팔레트 묶음 팔레트 ID | 공팔레트 묶음은 팔레트 ID를 관리한다. 제품의 시리얼(MST-SN)과 같은 방식으로 관리한다 |
| BR-HU-VAL-010 | 대표 팔레트 ID | 맨 밑 팔레트가 대표 팔레트 ID로 묶음의 논리 위치(invn_id)가 되고, 대표 팔레트 ID를 포함한 적재 팔레트 ID 전체를 추적 관리한다 |
| BR-HU-VAL-011 | 매거진 보관 후 AS/RS 보관 투입 | 매거진 배출 요청 시 현대무벡스가 팔레트 ID(대표 팔레트 ID와 적재 팔레트 ID)를 같이 보내고, WCS는 해당 팔레트의 AS/RS 적치 지시를 현대무벡스에 보낸다 (MST-EQP BR-MST-EQP-020) |
| BR-HU-VAL-012 | HU PACK·UNPACK·SWAP | WCS는 자동화 설비 재고에 대해 HU PACK, HU UNPACK, HU SWAP 기능을 제공한다 |
| BR-HU-VAL-013 | HU 작업 수행 방식 | 작업자가 WCS PDA에서 특정 셀의 재고현황을 조회해 PACK·UNPACK·SWAP을 처리한다 |
| BR-HU-VAL-014 | HU 검증·작업 API | EWMS가 HU 조회(HU 스캔 시), HU 생성, HU 삭제와 PACK, UNPACK, SWAP API를 제공하며, WCS PDA에서 처리할 때 WCS가 호출한다 (같은 날 PACK·UNPACK·SWAP API 추가로 변경) |
| BR-HU-VAL-015 | UNPACK 범위 | HU에서 재고 전부 또는 일부를 뺄 수 있다 |
| BR-HU-VAL-016 | HU 작업 셀 범위 | 작업자가 접근할 수 있는 셀로 한정하며, 접근 가능 여부는 셀 기준정보 설정을 기준으로 한다 |
| BR-HU-VAL-017 | HU와 invn_id | HU는 팔레트 ID이므로 팔레트 재고는 invn_id가 HU와 같다 (INV-UNT) |
| BR-HU-VAL-018 | 팔레트갈이 | 원 HU에서 HU UNPACK하고 대상 HU에 HU PACK하는 것으로 처리한다 |
| BR-HU-VAL-019 | HU 작업 API 실패 | EWMS API 호출이 실패하면 해당 작업을 처리하지 못하게 막는다(WCS 재고 미반영) |
| BR-HU-VAL-020 | 디베닝 팔레타이징 HU | 디베닝 팔레타이징 결과로 수집된 시리얼번호와 팔레트 ID로 EWMS가 HU 생성·PACK을 처리하도록 WCS가 API를 호출한다 (INB-DVN BR-INB-DVN-018) |
| BR-HU-VAL-021 | HU 시리얼정보 통보 | HU의 시리얼 정보가 바뀌면 EWMS가 자기 내역을 HU 단위 전체 목록으로 WCS에 통보한다. 시리얼은 유니크하므로 시리얼당 수량은 1이다 |
| BR-HU-VAL-022 | MIXED 출고 팔레트 ID·HU | 1층·3층 MIXED 출고 팔레트 ID는 공팔레트에 이미 붙은 팔레트 ID를 그대로 쓴다(WCS 채번 없음). 출고 구성 완료 보고를 받으면 WCS가 EWMS HU 생성·PACK을 호출하고, 출고확정 때 그 HU 기준으로 피킹 W/T를 확정한다 |

## 5. 요구사항
| ID | 요구사항 | 주체 | 상태 |
| --- | --- | --- | --- |
| REQ-HU-VAL-001 | WCS는 팔레타이징 유형별 HU 확인/생성 주체 규칙(BR-HU-VAL-003~006)에 따라 처리해야 한다. | WCS | 확인필요 |
| REQ-HU-VAL-002 | WCS는 HU 생성이 완료되지 않은 팔레트의 W/T 확정을 차단해야 한다. | WCS | 확인필요 |
| REQ-HU-VAL-003 | WCS는 팔레타이징 결과를 EWMS 처리 순서대로 실시간 또는 배치로 연동해야 한다. | WCS | 확인필요 |
| REQ-HU-VAL-004 | 공팔레트는 제품과 동일하게 재고로 관리하며, 팔레트 유형별 공팔레트 묶음 단위로 AS/RS에 보관·관리해야 한다 (공급 절차 MST-EQP). | WCS | 확정 |
| REQ-HU-VAL-006 | WCS는 공팔레트 팔레트 ID를 시리얼 마스터·이력 로그(MST-SN)와 같은 방식으로 관리해야 한다. 공팔레트 팔레트 ID 마스터와 이력 로그를 두고, 묶음 구성(대표 팔레트 ID ↔ 적재 팔레트 ID)을 함께 관리한다. | WCS | 확정 |
| REQ-HU-VAL-007 | 공팔레트 묶음 재고는 맨 밑 팔레트의 대표 팔레트 ID를 invn_id로 해 위치를 관리하고, 적재 팔레트 ID는 대표 팔레트 ID의 위치를 따른다. 대표 팔레트 ID를 포함한 적재 팔레트 ID 전체를 추적할 수 있어야 한다. | WCS | 확정 |
| REQ-HU-VAL-008 | WCS는 현대무벡스의 매거진 배출 요청 수신 시 현대무벡스가 보낸 대표 팔레트 ID와 적재 팔레트 ID로 묶음 구성을 등록하고 이력 로그를 쌓아야 한다. | WCS | 확정 |
| REQ-HU-VAL-009 | 공팔레트 이력 이벤트와 검증: ① 이벤트 = AS/RS 보관 투입(묶음 구성 등록), 묶음 피킹·공급(설비 도착), 설비 사용(팔레타이징 실적의 팔레트 ID로 제품 팔레트(HU) 전환 연결), 다시 매거진 배출 후 보관 투입 시 재구성. 이벤트마다 변경 TASK번호를 남긴다 ② 매거진 안의 팔레트 ID는 WCS가 관리하지 않으므로, 공급 후의 위치는 다음 보관 투입 통보나 팔레타이징 실적에서 다시 확정한다 ③ 묶음 재고 수량(장수)과 적재 팔레트 ID 수가 다르거나, 같은 팔레트 ID가 다른 묶음에 이미 있으면 알람을 보내고 수신값 기준으로 구성을 갱신한다(물류 진행은 막지 않음). | WCS | 확정 |
| REQ-HU-VAL-010 | WCS는 자동화 설비 재고에 대해 HU PACK, HU UNPACK, HU SWAP 기능을 제공해야 한다. | WCS | 확정 |
| REQ-HU-VAL-011 | WCS PDA는 특정 셀의 재고현황을 조회하고, 조회한 재고에 대해 HU PACK·UNPACK·SWAP을 처리할 수 있어야 한다. | WCS | 확정 |
| REQ-HU-VAL-012 | WCS는 HU 검증과 HU 작업 반영을 EWMS가 제공하는 HU 조회(HU 스캔 시)·HU 생성·HU 삭제·PACK·UNPACK·SWAP API로 처리하며, WCS PDA에서 작업을 처리할 때 해당 API를 호출해야 한다 (PACK·UNPACK·SWAP API 추가로 변경). | WCS→EWMS | 확정 |
| REQ-HU-VAL-013 | HU 작업 공통 절차: ① 접근 가능한 셀 스캔 → 셀 재고현황(invn_id, HU, 제품, LOT, 수량, 상태) 표시 ② 작업 가능 점검 — 예약수량·작업 잠금(진행 중 Task)·보류가 있으면 작업 불가 ③ HU 스캔 → HU 조회 API로 검증 ④ EWMS PACK·UNPACK·SWAP API 호출(REQ-HU-VAL-014~016) ⑤ API가 성공한 뒤 WCS 재고 이력을 한 번에 반영 — 팔레트 invn_id = HU이므로 HU가 바뀌는 수량은 원 invn_id 출고 이력 + 대상 invn_id 입고 이력을 같은 변경 TASK번호로 남기고(GTP 분할과 같은 방식, INV-UNT BR-INV-UNT-011), 시리얼 관리 제품은 시리얼 이력 로그에도 남긴다 ⑥ 작업자·시각·작업 유형·전후 HU를 작업 이력으로 남긴다. | WCS | 확정 |
| REQ-HU-VAL-014 | HU PACK: 셀 재고(HU 없는 재고 또는 다른 HU에서 UNPACK한 재고)와 수량을 고르고 대상 HU를 스캔한다. HU 조회 결과 없으면 HU 생성 API로 만든 뒤 PACK API를 호출한다. 이미 재고가 담긴 HU에도 추가로 담을 수 있다. WCS는 대상 HU(invn_id) 입고 이력으로 반영한다. | WCS | 확정 |
| REQ-HU-VAL-015 | HU UNPACK: 셀의 HU를 스캔해 확인하고 뺄 재고·수량을 고른다(전부 또는 일부). UNPACK API를 호출하고 WCS는 원 HU(invn_id) 출고 이력으로 반영한다. 팔레트갈이는 원 HU UNPACK + 대상 HU PACK을 한 작업(같은 변경 TASK번호)으로 묶어 처리하고, 원 HU가 비면 재고 행을 종료한다(HU 삭제 API 호출 여부 Q-154). | WCS | 확정 |
| REQ-HU-VAL-016 | HU SWAP: 기존 HU와 새 HU를 차례로 스캔한다. 새 HU는 HU 조회 결과 없거나 비어 있어야 하며(다른 재고가 담겨 있으면 거부), 없으면 HU 생성 API로 만든 뒤 SWAP API를 호출한다. WCS는 기존 invn_id 출고 + 새 invn_id 입고 이력으로 바꾸고, 수량·LOT·위치는 그대로다. | WCS | 확정 |
| REQ-HU-VAL-018 | 접근 가능 셀 관리: 셀 기준정보(MST-LOC)에 'HU 작업 가능' 여부를 두고, WCS PDA는 이 표시가 있는 셀만 조회·작업할 수 있게 한다. 표시가 없는 셀을 스캔하면 작업 불가 안내를 띄운다. | WCS | 확정 |
| REQ-HU-VAL-019 | WCS는 HU UNPACK에서 재고 전부 또는 일부를 뺄 수 있게 해야 한다. | WCS | 확정 |
| REQ-HU-VAL-020 | WCS PDA의 HU PACK·UNPACK·SWAP은 작업자가 접근할 수 있는 셀로 한정해야 한다. | WCS | 확정 |
| REQ-HU-VAL-021 | WCS는 HU 작업 중 EWMS API 호출이 실패하면 해당 PACK·UNPACK·SWAP을 처리하지 못하게 막고, WCS 재고에 반영하지 않아야 한다. | WCS | 확정 |
| REQ-HU-VAL-022 | WCS는 EWMS가 보내는 HU 시리얼정보 통보(HU 단위 전체 목록)를 받아 해당 HU(팔레트 invn_id)의 시리얼 정보를 EWMS 내역 기준으로 갱신해야 한다. | EWMS→WCS | 확정 |
| REQ-HU-VAL-023 | HU 시리얼정보 통보 반영 방식: ① 통보받은 HU가 WCS 재고(자동화 설비)에 있으면 HU 단위 전체 목록으로 시리얼 마스터를 맞추고(시리얼당 수량 1) 시리얼 이력 로그에 변경 전·후를 변경 TASK번호와 함께 남긴다(MST-SN) ② 시리얼 수가 WCS 재고 수량과 달라지면 WCS 재고는 바꾸지 않고 재고대사 대상(INV-SYNC)으로 등록해 파워유저에게 알린다 ③ WCS 재고에 없는 HU(수작업 영역 등)는 이력만 남기고 무시한다 ④ 같은 통보를 두 번 받으면 한 번만 반영한다. | WCS | 확정 |
| REQ-HU-VAL-024 | MIXED 출고 팔레트 HU 처리: ① 출고 팔레트 ID = 설비가 읽어 보고한 공팔레트 ID ② 출고 구성 완료 보고(출고 팔레트 ID, 적재 내역)를 받으면 EWMS HU 생성·PACK API를 호출한다(팔레트 ID, 제품, LOT, 수량, 시리얼번호 목록) ③ API 실패·EWMS 단절 시 설비는 멈추지 않고 대기열에 쌓아 복구 후 재전송하며, HU 생성이 성공해야 해당 팔레트의 출고확정을 할 수 있다 ④ 1층 GANTRY MIXED와 3층 MIXED에 같게 적용한다. | WCS | 확정 |

## 6. 인터페이스
| 송신→수신 | 시점 | 주요 항목 |
| --- | --- | --- |
| EWMS→WCS | HU의 시리얼 정보가 바뀔 때 | HU 시리얼정보 통보(EWMS 내역): HU(팔레트 ID), 제품, 제조번호(S/N) 전체 목록(HU 단위, 시리얼당 수량 1), 통보 일시 — 신규·변경·삭제 FLAG 여부 협의 (Q-174) |
| WCS→EWMS | HU 생성 (실시간/배치) | 팔레트 ID, 적재 내역 |
| WCS→EWMS | HU 조회 API (HU 스캔 시 PACK·UNPACK·SWAP 검증, 장기 보관 재고·GTP 잔량 HU 확인) | HU(팔레트 ID) → 존재 여부·내역 (응답은 API 호출 결과) |
| WCS→EWMS | HU 생성 API (PACK·SWAP 대상 HU가 없을 때) | HU(팔레트 ID), 적재 내역 |
| WCS→EWMS | HU 삭제 API (빈 HU 정리, 사용 시점 Q-154) | HU(팔레트 ID) |
| WCS→EWMS | PACK API (WCS PDA PACK 처리 시) | 대상 HU, 제품, LOT, 수량 (항목 Q-154) |
| WCS→EWMS | UNPACK API (WCS PDA UNPACK 처리 시, 전부·일부) | 원 HU, 제품, LOT, 수량 (항목 Q-154) |
| WCS→EWMS | SWAP API (WCS PDA SWAP 처리 시) | 기존 HU, 새 HU (항목 Q-154) |
| 현대무벡스 ECS→WCS | 매거진 꽉 참 (매거진 배출 요청) | 설비 ID, 대표 팔레트 ID(맨 밑), 적재 팔레트 ID 전체, 팔레트 유형, 수량(장수) |
| WCS→현대무벡스 ECS | 매거진 배출 요청 수신 후 (AS/RS 적치 작업지시) | 대표 팔레트 ID(묶음), 팔레트 유형 |

## 7. 예외 처리
- HU 사용 불가 팔레트: WCS는 HU 확인을 하지 않으며(EWMS 선행, INB-PLT BR-INB-PLT-023), 적치 W/T 없음은 Reject 라인 토출, HU 내역 없음은 적치 확정 보류·알람(INB-PLT REQ-INB-PLT-032)으로 처리
- HU PACK·UNPACK·SWAP 중 EWMS API 호출 실패: 작업을 처리하지 못하게 막고 WCS 재고는 반영하지 않는다 (BR-HU-VAL-019). 팔레트갈이처럼 API를 이어서 호출하다 중간에 실패한 경우(UNPACK 성공 후 PACK 실패) EWMS와 WCS 상태 정합은 미정 (Q-154)

## 8. 미결 사항
- Q-174: [추가 협의] HU 시리얼정보 통보에 신규·변경·삭제 등 FLAG를 붙일지 (EWMS 내역을 HU 단위 전체 목록으로 통보·시리얼당 수량 1·REQ-HU-VAL-023 채택 결정)
- Q-154: HU PACK·UNPACK·SWAP 남은 정리 — (1) [추후 정리] PACK·UNPACK·SWAP API 항목·응답, 빈 HU의 HU 삭제 API 호출 여부 (2) 팔레트갈이처럼 API를 이어서 호출하다 중간에 실패한 경우(UNPACK 성공 후 PACK 실패) EWMS·WCS 상태 정합 방법 (팔레트갈이 = UNPACK + PACK 확인, 접근 가능 셀 = 셀 기준정보 설정 기준, API 실패 시 처리 불가 제어, REQ-HU-VAL-013~016·018 채택)

---
[HU-VAL 끝 · 다음 입력: K30 OUB-BAT 출고배치관리]
