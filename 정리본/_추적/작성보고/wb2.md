# 작성 보고 — wb2

맡은 문서: B06 조직·재고관리체계, B07 창고 기초속성, B08 WCS 위치, B09 EWMS BIN 코드, B32 차량·도크

## 1. 문서별 통계 (재료 기록 수 / 출처로 쓴 uid 수 / 반송 수)

| 문서 | 재료 기록 수 | 재료 uid 수(#seq 합침) | 문서에 출처로 쓴 uid 수 | 반송 수 |
|---|---|---|---|---|
| B06 조직·재고관리체계 | 72 | 59 | 61 (B07 재료 K05-F04.00, wp1 재료 REQ-MST-STK-014 포함) | 0 |
| B07 창고 기초속성 | 19 | 18 | 36 (다른 담당 재료 uid 17개 포함 — 4절) | 0 |
| B08 WCS 위치 | 100 | 85 | 85 | 0 |
| B09 EWMS BIN 코드 | 81 | 60 | 65 (B06·B08 재료 uid 포함) | 0 |
| B32 차량·도크 | 45 | 38 | 38 | 0 |

- 재료 uid는 모두 맡은 문서 다섯 곳 중 어딘가의 `〔…〕`에 나온다(자체 점검 스크립트로 확인, 누락 0).
- 반송은 없다. 맡은 문서끼리 옮긴 기록은 4절에 있다.

## 2. 합친 중복 (대표 문장 요지 · uid 목록)

**B06**
- 창고와 화주가 최상위 관리 단위 · C01-4.01, BR-MST-CTR-001, K05-F03.00, K05-OV01, G129, G130, G145, BR-MST-OWN-001, K06-OV01, REQ-MST-OWN-001
- 기준정보·재고·작업지시는 창고 아래 · C01-4.01, BR-MST-CTR-001, G129, K05-OV02
- 화주는 공통 관리, 창고별로 두지 않음 · BR-MST-CTR-005, K05-F05.00, BR-MST-OWN-007, REQ-MST-PRD-008
- 센터코드↔창고코드 항상 맵핑 / 같은 코드 여부는 등록자 판단 / 창고는 PLANT–센터코드 맵핑으로 식별 · G041, BR-MST-CTR-006, BR-MST-STK-006, REQ-MST-CTR-008, C01-4.02
- PLANT를 화주로 맵핑해 식별 · G145, G220, BR-MST-OWN-003, K06-OV03
- 평택도 화주 구분 / 통합 여부는 맵핑 데이터로 · G145, BR-MST-OWN-008, BR-MST-STK-004, K07-OV03
- PLANT 관리(PLANT → 창고·화주 맵핑) · BR-MST-STK-005, BR-MST-STK-010, REQ-MST-STK-001, G137
- EWMS 수신 원본을 그대로 보관 · G136, BR-SYS-JOB-006, REQ-SYS-JOB-005, BR-MST-STK-008, REQ-MST-STK-009
- 사용자 권한별 화주 제한 · BR-MST-OWN-006, REQ-MST-OWN-005

**B07**
- 프로세스는 그 창고의 기초속성 변수를 읽어 동작을 정한다 · G039, K08-OV02, REQ-MST-OPT-002
- 변수 행마다 같은 변수를 말한 기록을 합쳤다. 예: AS/RS 적재율 알림 기준값 · BR-MST-OPT-003, BR-EQP-ASRS-010, REQ-EQP-ASRS-013 / BOX Shuttle 적치율 기준값 · BR-MST-OPT-003, BR-EQP-BSH-010, REQ-EQP-BSH-007, REQ-EQP-BSH-008, REQ-EQP-BSH-014

**B08**
- WCS는 자동화 설비 구역 재고만 관리 · BR-MST-LOC-006, BR-MST-LOC-011, REQ-MST-LOC-004, K11-OV06
- 수작업 영역 재고는 WCS가 관리하지 않음 · G003, G091, BR-MST-LOC-006, REQ-MST-LOC-004, BR-INV-WT-007, K11-OV07
- 출고 수작업 영역 재고는 WCS가 관리하지 않음 · BR-OUB-MAN-009, K33-OV02, REQ-OUB-MAN-006
- BIN 안의 상세 위치·상세 재고를 WCS가 관리 · BR-MST-LOC-007, BR-MST-LOC-012, REQ-MST-LOC-005, REQ-MST-LOC-008, K11-OV06
- GTP 출고와 OEM 3층 논리 출고 DOCK이 같은 출고 수작업 영역 · G093, G213, K33-OV02, BR-OUB-MAN-007, K33-F04.00, REQ-OUB-MAN-006
- 출고 수작업 영역은 층별 BIN 단위 셀 · BR-MST-LOC-015, BR-MST-LOC-034, BR-OUB-MAN-016, REQ-OUB-MAN-013, REQ-MST-LOC-013, BR-OUB-MAN-024
- 셀에서 상위 값을 덮어쓰지 않음 / 존·로케이션도 함께 확인 · BR-MST-LOC-032, REQ-MST-LOC-030, REQ-MST-LOC-031, BR-INV-QRY-011
- 셀 상태 값·뜻 · G108, BR-INV-QRY-002, REQ-INV-QRY-002, REQ-INV-QRY-004, REQ-INV-UNT-030
- 셀 유형 값·뜻 · G109, BR-MST-LOC-031, REQ-MST-LOC-029, REQ-MST-LOC-030
- 위치셀 코드 포맷 · G034, BR-MST-LOC-005, REQ-MST-LOC-002 / 셀 코드 = 설비 위치 코드 · BR-MST-LOC-005, BR-MST-LOC-039
- 셀 피킹 우선순위 · BR-MST-LOC-030, REQ-MST-LOC-028, BR-EQP-BSH-014
- HU 작업 가능 여부 · BR-MST-LOC-016, REQ-MST-LOC-014, REQ-HU-VAL-018
- 1층 수작업 영역은 입고 수작업 영역뿐 · K26-OV03, BR-OUB-MAN-005, K33-F03.00
- 4층 입고장 존재 · BR-EQP-ASRS-028, BR-MST-LOC-029
- 1층 출고 DOCK 22개 · G213, BR-YRD-VHC-023 / ATL DOCK 입출고 겸용 · G054, BR-YRD-VHC-023

**B09**
- 재고위치는 Stock Type·BIN 두 기준 · G035, BR-MST-LOC-026, REQ-MST-LOC-026
- 자동화 설비 구역을 BIN으로 크게 묶음 · BR-MST-LOC-007, BR-MST-LOC-012, K11-OV02
- 자동화 설비 재고 = 0050·WCS1 · G035, K11-OV03, BR-MST-LOC-028, REQ-MST-LOC-027, Q-161
- 구역별 Stock Type·BIN 표 · BR-MST-LOC-027, K11-OV09, G144 (+ 각 코드 용어 정의, G050, G065, BR-MST-LOC-029)
- 9011·9013은 설비 재고 영역 아님 · G035, G174, BR-MST-LOC-025, K11-OV04, REQ-MST-LOC-025
- 셀 단위 BIN 매핑·셀 값 우선 · G038, BR-MST-LOC-038, REQ-MST-LOC-037, REQ-MST-LOC-023
- BIN 변경 이동의 뜻 · G035, BR-MST-LOC-037, K11-OV06, REQ-MST-LOC-035, REQ-MST-LOC-023
- BIN 변경 이동은 W/T로만 반영 · G035, BR-MST-LOC-036, K11-OV06, REQ-MST-LOC-023, REQ-MST-LOC-034, BR-INV-SYNC-007 (BR-INV-SYNC-007은 「W/T로 반영된다」로 「만」이 없다. 뜻이 어긋나지 않아 합쳤다)
- RCV9 ↔ 위치셀 RCV9 · BR-MST-LOC-001, REQ-INB-DVN-017 / AS/RS 적치 셀 = WCS1 · BR-MST-LOC-021, K26-OV03, BR-INB-DVN-034
- 출고 수작업 셀 BIN · BR-OUB-MAN-016, REQ-OUB-MAN-013, BR-MST-LOC-015, BR-MST-LOC-034
- DEST 행선 · BR-MST-LOC-033, BR-INB-PLT-034
- RCV9+WCS1 = WCS 전체 재고 · BR-INV-SYNC-001, K37-OV01

**B32**
- 국내생산·국내OEM 입문계획 = EWMS · G141, G180, G183, BR-YRD-VHC-002, K40-OV03
- 해외생산·해외OEM(역수입) 입문계획 = FIS · G141, G181, G184, G185, BR-YRD-VHC-003, K40-OV03
- 출고 차량정보는 전부 EWMS · BR-MST-ORD-007, BR-YRD-VHC-007, K40-OV03
- 입차·출차는 게이트 관리 시스템 연동 · BR-YRD-VHC-004, K40-OV04, K40-F03.00, K40-F06.00
- ATL DOCK 센싱 · G054, BR-YRD-VHC-020, K40-F04.00, K40-F05.00
- DOCK 기준정보는 사용자가 WCS에 등록, EWMS와 같아야 함 · G211, BR-YRD-VHC-021, K40-OV08
- 야드 관리 정의 · G204, K40-OV01

## 3. 원문 불일치 (⚠ 표시한 곳 — 두 ID와 차이)

1. **센터코드↔창고코드 맵핑을 두는 곳 (B06 3-2)** — BR-MST-CTR-006: PLANT–센터코드 맵핑을 WCS PLANT 관리의 「WCS 맵핑 창고코드」로 등록 / REQ-MST-CTR-008(·REQ-MST-CTR-003): 창고 마스터의 「EWMS 창고 식별값 매핑」으로 변환. 맵핑이 PLANT 관리와 창고 마스터 두 곳에 있다.
2. **Storage Location 맵핑을 두는 곳 (B06 3-3)** — BR-MST-STK-010: PLANT 관리 항목은 PLANT코드·WCS 맵핑 화주사코드·WCS 맵핑 창고코드(Storage Location 없음) / REQ-MST-STK-014(wp1 P01 재료): PLANT 또는 Storage Location을 「맵핑 마스터(PLANT–센터코드–화주 맵핑)」에서 찾는다.
3. **Storage Location 맵핑의 대상 (B06 3-2)** — K07-OV01: EWMS PLANT·Storage Location → 「화주·WCS Storage Location」 / REQ-MST-STK-002 [확인필요]: EWMS (PLANT, Storage Location) → 「WCS (PLANT, Storage Location)」. 맵핑 대상이 화주인지 PLANT인지 다르다. 같은 뜻일 수도 있어 확인이 필요하다.
4. **출고 수작업 영역 로케이션 코드 (B08 6절)** — G093(C02) [추후 정리]: 「층별로 식별할 수 있는 로케이션 코드를 지정할 예정(약 3개)」 / BR-MST-LOC-015: 그 계획을 바꿔 BIN 단위 셀로 두고 세부 로케이션은 WCS가 관리하지 않기로 정리. C02가 이 정리를 따라가지 않았다.
5. **BIN 마스터 구역 열거에서 4층 입고장 빠짐 (B09 3-1)** — REQ-MST-LOC-026·REQ-MST-LOC-023: 4층 입고장(9014·RCV4)이 없음 / BR-MST-LOC-027·BR-MST-LOC-029·K11-OV09·G144: 4층 입고장 있음.
6. **BIN 매핑 단위 (B09 3-5)** — K12-F14.00 [확인필요]: 설비마다 대응 EWMS BIN을 둔다 / REQ-MST-LOC-037·BR-MST-LOC-038: BIN 매핑의 기준은 셀(로케이션 값은 기본값).
7. **차량 입문계획 EWMS 송신 대상 (B32 3-3)** — G186: 쉽백도 EWMS가 보냄 / G141·BR-YRD-VHC-002·K40-OV03: EWMS 송신 목록은 국내생산·국내OEM뿐.

- 지시받은 후보 둘(REQ-MST-CTR-008 ↔ BR-MST-CTR-006, REQ-MST-STK-014 ↔ BR-MST-STK-010)은 1·2번이다.

## 4. 반송·제외 (uid · 이유 · 보낼 곳)

**반송: 없음.**

**맡은 문서끼리 옮김** (지침 6: 자기 문서끼리는 직접 옮긴다)
- BR-MST-OPT-002 · 창고 기초속성의 정의 사실 · B06 재료 → B07
- K05-F04.00 · 관리 계층 2단 표의 한 줄 · B07 재료 → B06(관리 계층 표)
- Q-161 · 「GANTRY·AS/RS 모두 0050 → BIN 변경 없음」은 BIN 사실 · B08 재료 → B09
- REQ-YRD-VHC-025(#5) · DOCK별 이력 보관 기간은 창고 기초속성 변수 · B32 재료 → B07 변수 목록. 같은 uid의 나머지(이력 항목·점유)는 B32에 남겼다
- REQ-MST-STK-010 · 쪼갬. 수신 원본의 Work 연결·수신 일시는 B06, 보관 기간 변수는 B07 목록
- BR-MST-LOC-029 · 쪼갬. 「4층에도 입고장이 있다」는 B08, 9014·RCV4 짝은 B09
- BR-MST-LOC-015(#2)·BR-MST-LOC-034 · BIN 코드(9013·RCV3, 9014·RCV4)만 B09 셀별 매핑 표로 옮기고, 셀을 BIN 단위로 둔다는 사실은 B08에 남겼다
- REQ-MST-LOC-007 · 로케이션의 EWMS BIN 매핑 칸 값(RCV9·WCS1·AMVT·MMVT)은 B09, 칸 이름은 B08
- K07-OV01 · 쪼갬. 「Stock Type은 PLANT·Storage Location 조합」은 B09, 화주·Storage Location 맵핑은 B06

**다른 담당 재료에서 출처로 쓴 uid** (읽기만 함. 원래 문서의 출처로도 쓰일 중복 출처다)
- B07 변수 목록: G072, BR-OUB-MIX-009, REQ-OUB-MIX-010 (wb5 B22) · REQ-OUB-GNT-021, BR-OUB-GNT-013, BR-OUB-GNT-029 (wb6 B31) · REQ-OUB-GNT-033 (wp3 P23) · REQ-EQP-BSH-008 (wp2 P16) · REQ-EQP-BSH-014 (wp4 P27) · REQ-INV-RSV-015, Q-136 (wp5 P33) · REQ-INB-PLT-032 (wp1 P02) · REQ-MST-STK-012 (wp5 P35) · REQ-SYS-JOB-006 (wp6 P42) · BR-INV-SYNC-011, REQ-INV-SYNC-013 (wp5 P39)
- B06: REQ-MST-STK-014 (wp1 P01) — 원문 불일치 2번의 짝으로만 썼다

**제외: 없음.**

## 5. 추가로 지운 기술
- BR-MST-OWN-008·BR-MST-STK-004 · 「기능을 구현한다」 → 「평택 CDC도 화주를 구분한다」로 업무만 남김
- BR-YRD-VHC-016 · 「차량번호를 키로 찾는다」 → 「차량번호로 찾는다」
- REQ-MST-OWN-003 · 「맵핑을 조회해 보여 준다」 → 「맵핑에서 보여 준다」
- 그 밖에 재료에 남은 기술 단어(메소드·트랜잭션·테이블·API·invn_id·wh_id·파라미터·커밋)는 없었다. 「push」는 송신 방향(누가 누구에게)이라 B32 미결 줄에 남겼다.
- G136 「EWMS 수신 원본」은 정의 대부분이 기술이라 결정로그 §5대로 용어 표 비고에 「용어째 삭제 후보」를 달고 남겼다.

## 6. 비어 있는 칸 (묶음 · 칸)
- B06 · 사용 여부·이력 — 별도 사실 없음(운영 여부·등록·수정 이력 칸은 구성 항목에 있음) / 상태 값 — 해당 없음
- B07 · 사용 여부·이력 — 별도 사실 없음(칸은 구성 항목에 있음) / 상태 값 — 해당 없음 / 변수 기본값 — 로딩 옵티마이저 수행 주체, HU 내역 조회 응답 대기·재조회 횟수, PARTIAL 반영 순서, LOT 속성 창고 기본값, TRASH BIN 최소 보유 수량, 수신 원본 보관 기간은 원문에 없음
- B08 · 사용 여부·이력 — 사용 여부 칸만 있고 변경 이력은 원문에 없음
- B09 · 사용 여부·이력 — 사용 여부 칸만 있고 변경 이력은 원문에 없음 / 미결 — 원문에 없음
- B32 · 비어 있는 칸 없음

## 7. 다른 프로세스와 겹침 / 판단이 어려웠던 곳
- **실적 PLANT 규칙 겹침:** B06의 「EWMS 실적은 원래 수신 PLANT별로 나눠 보낸다」(C01-4.02, G137)는 wp5 P35 재료의 BR-MST-STK-007과 같은 규칙이다. 조건 없는 제약이라 B06에 두었다. P35에서 같은 문장을 다시 쓰면 겹친다.
- **사용자 권한(REQ-MST-CTR-005, BR-MST-OWN-006, REQ-MST-OWN-005):** 맡을 묶음이 없어 창고·화주가 범위 단위라는 뜻으로 B06 「그 밖의 사실」에 두었다. 대안은 「단말」 묶음이나 새 「사용자·권한」 묶음이다.
- **B07 HU 대기 시간 두 줄:** 「EWMS HU 확인 응답 대기 시간」(REQ-EQP-GNT-023, GANTRY 쪽)과 「HU 내역 조회 응답 대기 시간·재조회 횟수」(REQ-INB-PLT-032, 일반입고)는 같은 변수인지 원문으로 알 수 없어 행을 나눴다. 확인이 필요하다.
- **B07 변수 이름:** BR-MST-OPT-003은 BOX Shuttle 변수를 「적재율」로, K22 문서들은 「적치율」로 적는다. 숫자(80%)는 같아 불일치로 보지 않았다.
- **B07 미결 표시:** BR-MST-OPT-003 전체에 〈미결(Q-136)〉이 붙어 있지만, 원문 K08을 보면 Q-136은 예약 정리 기준 시간의 값에만 걸린다. 그래서 변수 행마다 상태를 따로 적었다.
- **B08 ↔ B09 경계:** 지시대로 BIN 코드가 붙은 매핑 사실(셀 → BIN, 출고 수작업 셀 BIN, 로케이션 BIN 매핑 값)은 모두 B09에 두었다. B08은 「EWMS BIN 단위 셀」, 「EWMS BIN 매핑 칸」처럼 BIN을 이름으로만 쓴다. 다만 B08의 「WCS는 EWMS BIN 안의 상세 위치를 관리한다」는 매핑이 아니라 WCS 재고 경계라 B08에 두었다.
- **B09 「BIN 변경 이동」:** 용어소속 표에 없는 말이다. 용어 표에 넣지 않고 3-6 사실 절에 뜻을 적었다. 용어로 올릴지는 2단계 판단이 필요하다.
- **B09 DEST 행선 표(BR-MST-LOC-033, BR-INB-PLT-034):** 값의 뜻으로 보아 묶음에 두었다. 「DEST가 ~면 ~로 보낸다」로 읽으면 P02 일반입고·P03 디베닝입고의 조건 규칙이다.
- **B08 GANTRY 호기 로케이션(REQ-MST-LOC-030):** K12의 GANTRY 설비 유형별 속성(호기 번호·용도)과 겹칠 수 있다. 「GANTRY」·「설비 공통·설비사」 묶음 담당과 맞춰 볼 곳이다.
- **출고 DOCK 코드 정합(REQ-YRD-VHC-023 #3):** 지시대로 출고 DOCK은 WCS 위치이므로 B08에 두었다. DOCK 기준정보 항목(DOCK 코드는 EWMS 코드 그대로)은 B32에 있다.
- **Q-177:** 미결 원문은 내 재료에 없다. B32의 [미결 Q-177] 표시는 BR-YRD-VHC-009·010의 상태를 따른 것이다.
