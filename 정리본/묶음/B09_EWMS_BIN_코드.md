# [묶음] EWMS BIN 코드

> 유형: 기준정보 · 확인 담당: 미정(§9-2) · 원문 기준일 2026-11-02
> 이 묶음은 EWMS가 재고위치를 관리하는 기준(BIN·Stock Type)과, WCS 셀을 BIN에 잇는 매핑에 대해 늘 참인 것만 담는다. 흐름·조건·순서는 프로세스 문서에 있다.

## 1. 범위
- EWMS BIN·Stock Type 코드와 구역별 짝, WCS가 가진 BIN 마스터, 셀 → BIN 매핑, BIN 변경 이동의 뜻을 다룬다.
- 「WCS1 ↔ 위치셀」 같은 매핑 사실은 이 묶음에만 둔다. 셀·로케이션 자체는 「WCS 위치」 묶음이 갖는다.
- BIN 변경 이동을 W/T로 반영하는 순서는 → P35 W/T 연동, BIN별 합산 대사는 → P39 재고대사에 있다.

## 2. 용어
| 용어 | 뜻 | 동의어 | 비고 |
|---|---|---|---|
| BIN | EWMS가 재고위치를 묶어 관리하는 단위(EWMS BIN: RCV1·RCV3·RCV9·WCS1·AMVT·MMVT). | EWMS 로케이션, Storage Bin | EWM 표준 Storage Bin과 달리 큰 구역 단위로 쓴다 — 프로젝트 고유라 정의를 둔다(§9-8). 문서 표기는 BIN으로 통일한다 |
| BIN 마스터 | WCS가 관리하는 EWMS BIN 기준정보. 층과 BIN의 맵핑을 담고, 셀이 어느 BIN에 속하는지 정한다. | | |
| Stock Type | EWMS가 BIN과 함께 쓰는 재고위치 관리 기준 코드. | S.TYPE | EWM 표준 Stock Type(재고 유형)과 달리 위치 기준 코드로 쓴다 — 프로젝트 고유라 정의를 둔다(§9-8) |
| 자동화구역 | EWMS가 자동화 설비 재고를 관리하는 구역. Stock Type 0050, BIN WCS1이다. | | |
| WCS1 | 자동화 설비 구역 재고의 EWMS BIN. 자동화구역의 BIN이며 Stock Type은 0050이다. | | |
| RCV1 | 1층 입고장의 EWMS BIN(Stock Type 9011). | | |
| RCV3 | 3층 출고대의 EWMS BIN(Stock Type 9013). 3층 출고대 2개에 도착한 팔레트(Reject·Bypass 등)의 EWMS 위치다. | 3층 입고장(이전 명칭) | |
| RCV4 | 4층 입고장(4층 수작업)의 EWMS BIN(Stock Type 9014). | | |
| RCV9 | 디베닝설비 입고장의 EWMS BIN(Stock Type 9015). | 디베닝입고장, RCV19(이전 명칭), DVN1(이전 명칭, 사용 안 함) | |
| AMVT | 수작업 → 자동화 전환 EWMS BIN. 수작업 → 자동화 보충용 임시구역이며 Stock Type은 0300이다. | | |
| MMVT | 자동화 → 수작업 전환 EWMS BIN. 자동화 → 수작업 보충용 임시구역이며 Stock Type은 0300이다. | | |
| 9011 / 9013 | 1층 입고장(9011)·3층 출고대(9013)의 EWMS Stock Type 코드. BIN은 각각 RCV1·RCV3이다. | 9011, 9013 | |

- AMVT·MMVT는 수작업 구역과 자동화 구역 사이의 재고 이동을 처리하는 EWMS 전환 로케이션이다.

## 3. 사실

### 3-1. 무엇을 식별하나 (식별 기준)
- EWMS는 재고위치를 Stock Type과 BIN 두 기준으로 관리한다.
- EWMS가 로케이션이라 부르는 단위는 BIN이다.
- EWMS는 자동화 설비 구역을 BIN(로케이션) 단위로 크게 묶어 관리한다.
- EWMS BIN은 층 개념을 포함한다.
- AS/RS·GANTRY 등 자동화 설비 재고는 자동화구역(Stock Type 0050·BIN WCS1)이다.

**구역별 Stock Type · BIN**

| 구역 | Stock Type | BIN |
|---|---|---|
| 1층 입고장 | 9011 | RCV1 |
| 3층 출고대 | 9013 | RCV3 |
| 4층 입고장(4층 수작업) | 9014 | RCV4 |
| 디베닝설비 입고장 | 9015 | RCV9 |
| 자동화구역 | 0050 | WCS1 |
| 수작업 → 자동화 보충용 임시구역 | 0300 | AMVT |
| 자동화 → 수작업 보충용 임시구역 | 0300 | MMVT |

- ⚠ 원문 불일치: REQ-MST-LOC-026과 REQ-MST-LOC-023은 구역을 1층 입고장·3층 출고대·디베닝설비 입고장·자동화구역·보충용 임시구역 2개로 열거하고, 4층 입고장(9014·RCV4)을 넣지 않았다. BR-MST-LOC-027·029, K11-OV09, G144는 4층 입고장을 넣는다.

**구역별 뜻**
- Stock Type 9011(1층 입고장·RCV1)·9013(3층 출고대·RCV3)은 설비 재고 영역이 아니다. 설비를 태워 이동시킬 작업지시(W/T 출발·도착 위치 등)에만 쓴다.
- 디베닝입고의 EWMS BIN은 RCV9(디베닝입고장)이다.
- 디베닝설비 입고장(Stock Type 9015·BIN RCV9)은 자동화 BIN 안이다.
- 9011 → 9015 전환은 EWMS가 처리한다.
- AMVT·MMVT는 EWMS BIN이다. WCS는 BIN 마스터 맵핑만 갖고, 별도 재고 위치로 두지 않는다.

**W/T 목적지(DEST: Stock Type·BIN)가 가리키는 WCS 행선**

| DEST | WCS 행선 |
|---|---|
| 9013·RCV3 | 3층 수작업(3층 출고대) |
| 9014·RCV4 | 4층 수작업(4층 입고장) |
| 0050·WCS1 | AS/RS 보관 |
| 9015·RCV9 | 디베닝 입고 후 팔레타이징해 AS/RS 보관 |

### 3-2. 구성 항목
- BIN 마스터 항목: 창고, 구역명, Stock Type, BIN 코드, 층(입고장·출고대만), 구분, 사용 여부. 구분 값: 입고장·출고대(1층 입고장 9011·RCV1 / 3층 출고대 9013·RCV3 / 디베닝설비 9015·RCV9), 자동화구역 0050·WCS1, 보충용 임시구역 0300·AMVT·MMVT.
- BIN 마스터에 구역별 Stock Type을 함께 둔다.
- 로케이션 기준정보의 EWMS BIN 매핑 칸에는 RCV9·WCS1·AMVT·MMVT를 쓴다.

### 3-3. 누가 만들고 어디서 오나
- Stock Type은 EWMS에서 PLANT와 Storage Location 조합으로 만든다.
- WCS도 EWMS BIN 마스터를 기준정보로 가진다. 층과 BIN의 맵핑이 필요한 처리에 쓴다.
- BIN 마스터를 쓰는 곳: BIN 변경 이동 판단(층간 이동 포함), EWMS 연동 BIN 지정, 재고대사.
- [확인필요] WCS는 EWMS BIN ↔ WCS 위치셀 매핑 마스터를 관리한다.

### 3-4. 사용 여부·이력
- BIN 마스터는 사용 여부를 갖는다(3-2). 변경 이력은 원문에 없음.

### 3-5. 셀 → BIN 매핑
- EWMS BIN이 층 개념을 포함하므로, WCS는 셀을 EWMS Stock Type·BIN과 매핑한다.
- BIN 매핑의 기준은 셀이다. 로케이션에 둔 BIN 매핑은 하위 셀이 물려받는 기본값이고, 셀에 따로 지정한 값이 있으면 셀 값을 쓴다.
- W/T DEST BIN 지정, BIN 변경 판단, 재고대사 BIN 합산은 모두 셀의 유효 BIN을 쓴다. 기준이 섞이면 BIN 판단이 달라져 W/T 확정이 실패할 수 있다.
- ⚠ 원문 불일치: [확인필요] K12-F14.00은 설비마다 자동화 설비 구역 여부(WCS 재고 관리 대상 구역 여부)와 대응 EWMS BIN을 둔다고 적는다(예: Y / WCS1). 매핑 기준을 셀로 정한 REQ-MST-LOC-037·BR-MST-LOC-038과 다르다.

**셀별 매핑**

| WCS 셀 | Stock Type · BIN |
|---|---|
| 자동화 설비 재고 셀(AS/RS·GANTRY·BOX Shuttle) | 자동화구역 0050·WCS1 |
| AS/RS 적치 셀 | 자동화구역 0050·WCS1 |
| WCS 자동화 구역(설비 위치셀) | WCS1 |
| 입고장·출고대 셀 | 해당 BIN(9011·RCV1 / 9013·RCV3 / 9014·RCV4 / 9015·RCV9) |
| 위치셀 RCV9 | RCV9(디베닝설비 입고장, Stock Type 9015) |
| 자동화 진입 위치셀 | AMVT |
| 자동화 배출 위치셀 | MMVT |
| 3층 출고 수작업 셀 | 9013·RCV3 |
| 4층 출고 수작업 셀 | 9014·RCV4 |

- 설비 재고 영역 밖의 입고장 셀은 해당 층 입고장으로 정한다.
- WCS는 AS/RS 셀을 WCS1로 합산하므로, BIN 대사에서 AS/RS 셀을 따로 맞출 필요가 없다.

### 3-6. BIN 변경 이동
- EWMS 위치 변경 여부는 층이 아니라 셀–BIN 매핑으로 판단한다.
- BIN 변경 이동: 출발 셀과 목적 셀의 BIN(Stock Type·BIN)이 다른 이동이다. 층간 이동도 BIN 변경 이동에 들어간다.
- AS/RS 1층 투입 → 3층 배출(0050 → 9013)도 BIN 변경 이동이다.
- GANTRY ↔ AS/RS 이동은 같은 자동화구역이라 BIN 변경 이동이 아니다.
- GANTRY 장기 보관 재고를 AS/RS에 투입해도 BIN 변경이 없다.
- BIN 변경 이동(층간 이동 포함)은 W/T로만 EWMS에 반영한다. WCS는 층간이동지시를 보내지 않는다.

### 3-7. WCS 재고 범위와 BIN
- EWMS의 RCV9·WCS1 수량 합과 WCS 전체 재고 수량은 늘 같아야 한다.

## 4. 상태 값
- Stock Type 값은 3-1 「구역별 Stock Type · BIN」 표에 있다.

## 5. 미결
원문에 없음.

## 6. 변경 이력
- BR-MST-LOC-025(9011·9013은 설비 재고 영역이 아니다)가 BR-MST-LOC-017을 대체했다.
- BR-MST-LOC-036(BIN 변경 이동은 W/T로만 반영)이 BR-MST-LOC-018·024를 대체했다.
- BR-MST-LOC-037(EWMS 위치 변경은 BIN으로 판단)이 BR-MST-LOC-020을 대체했다.
