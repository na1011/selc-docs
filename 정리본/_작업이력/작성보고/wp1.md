# 작성 보고 — wp1

> 맡은 문서: P01 EWMS 작업정보 수신 · P02 일반입고 · P03 디베닝입고 · P04 3층 수작업 이동 · P05 HU 확인·생성 · P06 HU PACK·UNPACK·SWAP · P07 수작업 영역 팔레트 운반
> 파일: `정리본/프로세스/P01_EWMS_작업정보_수신.md` ~ `P07_수작업_영역_팔레트_운반.md`

## 1. 문서별 통계 (재료 기록 수 / 출처로 쓴 uid 수 / 반송 수)

| 문서 | 재료 기록 | 재료 uid(중복 seq 합침) | 출처로 쓴 uid | 반송 | 비고 |
|---|---|---|---|---|---|
| P01 | 32 | 31 | 31 | 0 | |
| P02 | 100 | 94 | 94 (+1: P05에서 옮긴 K29-EX01) | 0 | 부분 반송 2건(아래 4절) |
| P03 | 111 | 101 | 101 | 0 | |
| P04 | 48 | 44 | 44 | 0 | |
| P05 | 23 | 23 | 17 | 0 | 6건은 맡은 문서끼리 옮김(P06 5건, P02 1건) |
| P06 | 32 | 32 | 32 (+5: P05에서 옮긴 것) | 0 | |
| P07 | 11 | 10 | 10 | 0 | |

## 2. 합친 중복 (대표 문장 요지 · uid 목록)

- **P01** 미등록·비운영 창고 → 처리 안 함·오류 이력·인터페이스 오류 알람 · REQ-MST-CTR-006, K05-EX01
- **P01** PLANT·SLoc 미맵핑 → 보류·오류 이력·알람 · REQ-MST-STK-014, K06-EX01, K07-EX01
- **P01** 출고예정 새벽 1회 PUSH · BR-MST-ORD-023, REQ-MST-ORD-021, K16-IF02
- **P01** 화주는 맵핑, LOT는 할당 전략으로 작업지시 시점 · BR-MST-ORD-024, REQ-MST-ORD-022
- **P01** 미등록 거래처 피킹 W/T → 수신·모니터링·작업지시 미생성 · BR-MST-CUS-008, K09-EX02, K09-F00.04, REQ-MST-CUS-010, REQ-MST-CUS-018
- **P01** GANTRY 호기 매핑 없음 → 보류 · BR-MST-CUS-015, K09-EX01, REQ-MST-CUS-018
- **P01** 등록 시 자동 재처리·파워유저 수동 재처리 · BR-MST-CUS-015, K09-EX01, K09-EX02, REQ-MST-CUS-018
- **P02** 적치 W/T 실시간 수신 · K26-F00.01, BR-INB-PLT-013, BR-INB-PLT-020, REQ-INB-PLT-020, BR-INV-WT-010
- **P02** 적치 W/T 수신 시 WCS 재고 등록(비가용·RCV1·LOT 채번) · K26-F00.01, BR-INV-WT-001, BR-INV-WT-010, BR-INB-PLT-014, BR-INB-PLT-026, REQ-INB-PLT-014, REQ-INB-PLT-025
- **P02** 목적지 문의(W/T 생성 요청 아님) · K26-F00.05, BR-INB-PLT-002, REQ-INB-PLT-002, BR-INB-PLT-013
- **P02** HU 내역 조회 → 적치 W/T 연결 → W/T DEST로 목적지 · K26-F00.06, BR-INB-PLT-021, REQ-INB-PLT-021, G119, G063, BR-INB-PLT-024, K26-OV02
- **P02** WCS는 HU 확인·생성 안 함(EWMS 선행) · BR-INB-PLT-023, REQ-INB-PLT-023, G063, K26-F00.06, K29-EX01
- **P02** DEST Stock Type·BIN으로 목적지 · BR-INB-PLT-034, REQ-INB-PLT-030, K26-IF02
- **P02** 사이즈 체크 규격 초과 Reject · BR-INB-PLT-001, K26-F00.04, REQ-INB-PLT-001
- **P02** Reject → 사유코드·AS/RS 미보관·3층 출고대 배출 · BR-INB-PLT-009, REQ-INB-PLT-009, K26-F01.03
- **P02** 투입 후 Reject는 3층 출고대 배출만 · BR-INB-PLT-010, REQ-INB-PLT-011, G087, BR-INB-PLT-032
- **P02** 토출건 DEST BIN 변경 확정 · BR-INB-PLT-017, REQ-INB-PLT-017, BR-INB-PLT-012, REQ-INB-PLT-013
- **P02** 팔레트 유형 불일치 Reject 확정 / W/T 없음 Reject 미확정 · BR-INB-PLT-028, REQ-INB-PLT-027, BR-INB-PLT-033, REQ-INB-PLT-024
- **P02** 1층 → 3층 Bypass는 적치 W/T · BR-INB-PLT-018, REQ-INB-PLT-018
- **P02** Reject 사유코드 양방향 · REQ-INB-PLT-026, BR-INB-PLT-027, BR-INB-PLT-030, G082
- **P02** 투입 전 오류 후진 배출 · BR-INB-PLT-032, REQ-INB-PLT-029, G087, K26-EX03
- **P02** 적치 W/T 없음 → Reject 라인 토출 · BR-INB-PLT-025, K26-F00.07, REQ-INB-PLT-024
- **P02** HU 응답 지연 → W/T DEST로 먼저 응답·재조회 · REQ-INB-PLT-032, BR-INB-PLT-036, K26-EX04, K29-EX01
- **P02** 적치 W/T 변경·취소 · REQ-INB-PLT-031, BR-INB-PLT-035, K26-EX05, REQ-INB-PLT-025
- **P03** 9011 → 9015·RCV9 전환(EWMS 처리) · BR-INB-DVN-032, K27-OV02, G175, BR-INB-DVN-016, K27-F00.01, G066
- **P03** 하역 전 디베닝 대상만 구분해 하역 대상 전송 · K27-F00.01, BR-INB-DVN-015, BR-INB-DVN-016, REQ-INB-DVN-013, REQ-INB-DVN-014
- **P03** 수신 시 팔레트 ID 없는 재고·RCV9·LOT 채번 · K27-F00.01, BR-INB-DVN-017, BR-INB-DVN-020, REQ-INB-DVN-014, REQ-INB-DVN-018, REQ-INB-DVN-017, BR-INB-DVN-032, G066, G175, K27-OV02
- **P03** 하역 대상을 로지스올에도 전달 · K27-F01.00, BR-INB-DVN-009, REQ-INB-DVN-007, G066
- **P03** 경로별 수량 사전 계산·전달(P-02) · REQ-INB-DVN-024, BR-INB-DVN-030, BR-INB-DVN-003, G066, BR-EQP-BSH-015, BR-EQP-VND-023, K22-F01.01, REQ-EQP-BSH-012, K27-IF03, K22-IF01
- **P03** 팔레타이징 결과로 HU 생성·PACK 요청 · K27-F01.07, BR-INB-DVN-018, BR-INB-DVN-022, BR-INB-DVN-006, BR-HU-VAL-020, REQ-INB-DVN-015, REQ-INB-DVN-020, BR-INV-WT-011, REQ-INB-DVN-017
- **P03** HU 성공 시 적치 W/T 생성 요청 · K27-F01.08, BR-INB-DVN-019, BR-INB-DVN-022, REQ-INB-DVN-016, REQ-INB-DVN-020, BR-INV-WT-011, REQ-INB-DVN-017, K18-IF01, K27-IF10
- **P03** 9015·RCV9 단일 적치 W/T, AS/RS 적치 후 확정 · BR-INB-DVN-021, REQ-INB-DVN-019, BR-INB-DVN-032, G175
- **P03** P-01 도착만 받음 · BR-EQP-VND-022, BR-INB-DVN-024
- **P03** 경로 A BOX 단위 적치 W/T 생성·확정 · REQ-EQP-BSH-013, BR-EQP-BSH-016, REQ-EQP-BSH-002, K22-F01.04, K22-IF04, REQ-INB-DVN-017
- **P03** BCR REJECT 판정은 로지스올 · REQ-INB-DVN-008, REQ-INB-DVN-010, G082, K27-EX01
- **P03** REJECT BOX는 RCV9에 그대로, 배출 위치 표시, BCR 앞 재투입 · BR-INB-DVN-026, REQ-INB-DVN-022, K27-EX02
- **P03** LEQ 미달 팔레트 마감 · BR-INB-DVN-031, REQ-INB-DVN-025, K27-EX03
- **P03** Product Code 불일치 Reject · REQ-MST-PRD-002, K13-EX01
- **P04** PDA 선행작업 → W/T 생성 → 이동 작업(W/T 생성 요청 안 함) · BR-INB-MAN-015, REQ-INB-MAN-016, K28-F01.00, K28-OV04
- **P04** 도착 정보 전송·W/T 확정 · K28-F03.01, BR-INB-MAN-012, REQ-INB-MAN-013, K28-OV04, K28-IF06
- **P04** WCS 재고 미등록 · BR-INB-MAN-010, REQ-INB-MAN-011, K28-OV03, BR-INV-WT-001
- **P04** OEM 3층 이동은 AS/RS 미경유 · BR-INB-MAN-013, BR-INB-PLT-019, REQ-INB-MAN-014, K28-F02.00
- **P04** 이동 수단 Workflow 기본·대체 · BR-INB-MAN-014, REQ-INB-MAN-015
- **P04** 쉽백 3층 수작업 이동 작업지시 필요 여부(Q-165) · Q-165, BR-MST-ORD-015, K16-F08.00, REQ-MST-ORD-016, BR-INB-MAN-002, K28-OV01
- **P05** WCS HU 확인 불필요(선행) · BR-HU-VAL-006, BR-INB-DVN-006
- **P05** 결과 연동은 EWMS 처리 순서대로 · BR-HU-VAL-002, K29-F02.00, REQ-HU-VAL-003
- **P06** 자동화 설비 재고 PACK·UNPACK·SWAP, PDA 셀 재고 조회 · BR-HU-VAL-012, BR-HU-VAL-013, K29-OV04, REQ-HU-VAL-010, REQ-HU-VAL-011, BR-SYS-UI-005, REQ-SYS-UI-014, G122, G123, G124
- **P06** 접근 가능 셀로 한정 · BR-HU-VAL-016, K29-OV05, REQ-HU-VAL-020, G122, G123, G124
- **P06** UNPACK 전부·일부 · REQ-HU-VAL-015, BR-HU-VAL-015, REQ-HU-VAL-019
- **P06** EWMS 실패 시 처리 불가·재고 미반영 · BR-HU-VAL-019, K29-OV06, REQ-HU-VAL-021, K29-EX02
- **P07** PDA 요청·W/T 미생성 · REQ-MST-LOC-011, G037, BR-MST-LOC-014, REQ-SYS-UI-010
- **P07** 대상 설비·경로는 Workflow · BR-MST-LOC-040, REQ-MST-LOC-039
- **P07** 오류·Reject는 재고 작업과 같은 절차 · REQ-MST-LOC-012, K11-EX02, BR-MST-LOC-014, G037

## 3. 원문 불일치 (⚠ 표시한 곳 — 두 ID와 차이)

| # | 문서 | 한쪽 | 다른 쪽 | 차이 |
|---|---|---|---|---|
| 1 | P03 2-1 | REQ-INB-DVN-002 (BCR 스캔 정보로 재고 생성) | BR-INB-DVN-020, REQ-INB-DVN-017 ① (하역 대상 수신 때 RCV9 재고 생성) | 디베닝 재고 생성 시점 |
| 2 | P05 3절 | BR-HU-VAL-006 (WCS는 HU 확인 불필요) | BR-HU-VAL-022, REQ-HU-VAL-024, BR-INB-DVN-018(P03) (WCS가 HU 생성·PACK 요청) | 같은 팔레타이징 유형에서 WCS의 HU 역할. 확인과 생성은 다른 단계이나 원문은 둘의 관계를 적지 않음 |
| 3 | P01 3절 C | K16-IF01 (입고 유형 4가지: 해외생산·국내생산·국내OEM·해외OEM) | REQ-MST-ORD-017 (대상: 역수입(법인/OEM)·STO·국내OEM) | 입고예정정보 수신 대상 범위(국내생산·STO)와 표기 |
| 4 | P02 2-1 | REQ-INB-PLT-005 〈확인필요〉 (투입 시점에 실시간 수신해 입고 처리) | REQ-INB-PLT-025 (적치 W/T 수신 시 재고 생성) | 입고 처리 시점 |
| 5 | P02 3절 Reject | K26-OV02 #4, BR-INB-PLT-016, G082 (1층 입고대 Reject는 ECS가 판단) | BR-INB-PLT-027, REQ-INB-PLT-026 (WCS가 찾는 Reject도 있음) | Reject 판단 주체 범위 |
| 6 | P03 2-2 | REQ-INB-DVN-006 (WCS가 1층 → 3층 이송·팔레트화 지시) | K27-F01.01 (하역 로봇 → 3층 이송 컨베이어, 로지스올 내부) | 1층 → 3층 이송 지시 주체(재료 doubt의 「모순 가능」) |
| 7 | P03 3절 | G175 #3 (디베닝 DEST 9015·RCV9 단일 적치 W/T) | G035 (AS/RS에 넣는 입고는 자동화구역 지정 — 내 재료 밖, 「EWMS BIN 코드」 쪽) | 디베닝 예외인지 확인 필요. G035는 출처로 달지 않고 본문에 이름만 적음 |
| 8 | P04 1절 | BR-INB-MAN-001, K28-OV02 (W/T 행선지 3층) | K28-OV01, BR-INB-MAN-010 (3층·4층) | 행선지 4층 W/T가 대상인지 |
| 9 | P04 3절 경로 | REQ-INB-PLT-019 (OEM 3층 입고를 Bypass로 처리하지 않음) | BR-INB-PLT-019, BR-INB-MAN-013 (EWMS가 Bypass 지시하면 업무 구분 없이 따름) | EWMS가 OEM 건에 Bypass를 지시할 때의 처리 |

⚠는 달지 않았지만 표기가 엇갈리는 곳:
- 「역수입」이 K16-F05.00에서는 해외생산(P02), K16-F07.00에서는 해외OEM(P04)에 붙어 있다. REQ-MST-ORD-017은 「역수입(법인/OEM)」이다. 각 문서에 원문 표기 그대로 두었다. 「입고 오더」 묶음의 역수입 정의와 맞춰 볼 필요가 있다.

## 4. 반송·제외 (uid · 이유 · 보낼 곳)

기록 전체를 반송한 것은 없다. 기록은 쓰고, 문장 안에서 다른 묶음의 늘 참인 사실을 설명하는 부분만 뺀 것:

| uid | 뺀 부분 | 이유 | 보낼 곳 |
|---|---|---|---|
| K26-IF01 | 「팔레트 ID(팔레트 유형 식별)」의 괄호 | 팔레트 ID가 유형을 식별한다는 것은 팔레트의 사실 | B16 팔레트 |
| K26-IF06 | 「에러 코드(설비사 원래 코드, WCS가 공통 사유코드로 맵핑)」의 괄호 | 설비사 코드 → 공통 사유코드 맵핑은 에러코드의 사실 | B05 에러·사유코드 |
| K26-F00.10 | 「유인지게차로 이동 (WCS 작업지시 없음)」의 괄호 | 유인지게차가 WCS 작업지시를 받지 않는다는 것은 운반 설비 사실(C01 §3) | B25 운반 설비 |

맡은 문서끼리 옮긴 것(지침 6: 직접 옮김):

| uid | 재료 | 옮긴 곳 | 이유 |
|---|---|---|---|
| G122, G123, G124 | P05 | P06 1절·3절 | HU PACK·UNPACK·SWAP 기능과 셀 범위 |
| BR-SYS-UI-005, REQ-SYS-UI-014 | P05 | P06 1절·6절 | 같은 이유(PDA PACK·UNPACK·SWAP) |
| K29-EX01 | P05 | P02 3절 HU·5절 | 일반입고 예외의 재진술(REQ-INB-PLT-032·BR-INB-PLT-023과 같은 사실) |

## 5. 추가로 지운 기술 (uid · 지운 내용)

| uid | 지운 내용 | 남긴 표현 |
|---|---|---|
| REQ-INB-DVN-017 #3 | 「API 실패」, 「EQP-GNT REQ-EQP-GNT-023 원칙과 같음」 참조 | EWMS 처리 실패·단절 때 설비는 멈추지 않고 복구 후 처리 |
| K29-OV06 | 「EWMS API 호출이 실패하면」 | EWMS 처리 실패 |
| BR-HU-VAL-019, REQ-HU-VAL-021, K29-EX02 | 「EWMS 호출」 | EWMS 처리 실패 |
| Q-154, K29-EX02 | 「이어서 호출하다」 | 이어서 요청하다 |

## 6. 비어 있는 칸 (문서 · 칸)

- P02 · 6. 기능·화면 — 별도 기능 기록 없음(3층 출고대 PDA·통합 Reject 화면 표시는 5. 예외 안에 있음)
- P04 · 5. 예외 — AMR 가동 불가 전환(3절) 외 원문에 없음
- P05 · 6. 기능·화면, 7. 미결 — 해당 기록 없음
- P01·P05·P06·P07 · 8. 변경 이력 — 해당 기록 없음
- P07 · 7. 미결 — 해당 기록 없음

## 7. 다른 프로세스와 겹침 / 판단이 어려웠던 곳

### 다른 프로세스와 겹침 (자기 문서에 쓰고 지우지 않음)

| 내 문서 · uid | 겹치는 프로세스 | 내용 |
|---|---|---|
| P01 · BR-MST-CUS-008·015, K09-EX01·02, K09-F00.04, REQ-MST-CUS-010·011·018 | P18 GANTRY 호기 운영 | 거래처 정보 오류(GANTRY 호기 매핑 없음). 재료 doubt의 대안이 P18 |
| P01 · BR-MST-ORD-023, K16-F09.00 | P22, P27 | 출고예정을 사전 보충이 씀 |
| P02 · K26-F00.11, BR-INB-PLT-014, REQ-INB-PLT-014 | P12 AS/RS 용량 부족 배출 | 용량 부족 배출 W/T 확정·재고 차감 |
| P02 · BR-INB-PLT-018, REQ-INB-PLT-018 | P10 AS/RS Bypass 이송 | 1층 → 3층 적치 W/T / 4층 → 3층 이동 W/T |
| P02 · BR-INB-PLT-012, K26-EX02 | P11 3층 출고대 도착 팔레트 처리 | 3층 도착 후 WCS PDA 판단(REQ-EQP-ASRS-016) |
| P02 · REQ-EQP-VND-012, BR-EQP-VND-024 | P17 설비 접점 인계 | ATL → AMR 이송, 목적지 Workflow |
| P02 · REQ-INB-PLT-022 / P03 · REQ-INB-DVN-002·017 | P37 제조번호 수신·추적 | 시리얼 마스터·이력 |
| P03 · K22-F01.03, REQ-EQP-BSH-012·013, BR-EQP-BSH-016 | P16 BOX Shuttle 적치·인출 | BOX Shuttle 투입·적치 공통 단계 |
| P03 · REQ-EQP-VND-011, BR-EQP-VND-022·023, BR-INB-DVN-030 | P17 설비 접점 인계 | P-01·P-02 인계 방식 |
| P03 · BR-INB-DVN-008 | P34 재고 반영·보정 | 자동화 BIN 안 관리단위 전환은 W/T 없음(BR-INV-UNT-011) |
| P03 · REQ-INB-DVN-017 #3 | P23 GANTRY 출고 | EWMS 실패 시 설비 무정지(REQ-EQP-GNT-023) |
| P04 · BR-INB-MAN-013, BR-INB-PLT-019 | P10 AS/RS Bypass 이송 | EWMS가 Bypass 지시 시 |
| P05 · BR-HU-VAL-022, REQ-HU-VAL-024 | P23, P28, P31 | MIXED 출고 팔레트 HU 생성·PACK, 출고확정 |
| P05 · BR-HU-VAL-007, K29-IF03 | P29 수작업 GTP 출고, P19 | 분할 팔레트 잔량·장기 보관 재고 HU 확인 |
| P06 · REQ-HU-VAL-013 | P29 수작업 GTP 출고 | 「GTP 분할과 같은 방식」 이력 |
| P07 · REQ-MST-LOC-012 | P44 설비 에러·Reject 처리 | 오류 처리 절차 |

### 판단이 어려웠던 곳

1. **미결 기록이 확정 내용의 재진술을 함께 담은 경우.** K26-EX03(Q-027), K27-EX02(Q-029·Q-169), K29-EX02(Q-154), BR-INB-DVN-027(Q-163)은 확정 기록과 같은 내용을 되풀이하고 미결 부분을 덧붙였다. 미결 표시는 **미결 부분을 적은 줄에만** 달고, 재진술 줄에는 출처로만 함께 달았다. 지침 4의 「가장 약한 상태」를 엄격히 적용하면 확정 줄에도 [미결]을 붙여야 한다. 검증 단계에서 판단해 주기 바란다.
2. **3층 도착 팔레트의 유인지게차 이동(P04).** K28-F03.02의 doubt는 운반 설비 사실의 중복이라 했다. 하지만 REQ-INB-MAN-010(확정)은 「WCS는 3층 도착 팔레트 이동에 작업지시를 내리지 않고 출발·도착 신호만 받는다」를 이 프로세스의 범위로 정한다. 그래서 프로세스 끝 경계로 남겼고, 「유인지게차는 WCS 작업지시를 받지 않는다」는 설명은 붙이지 않았다.
3. **REQ-HU-VAL-018 #2의 「이 표시」.** 재료만으로는 무엇을 가리키는지 알 수 없어 원문 K29를 열었다. seq1(「WCS 위치」 묶음 쪽)의 「HU 작업 가능」 표시를 가리킨다. P06에는 표시 이름만 썼다.
4. **BR-HU-VAL-015 (UNPACK은 전부·일부).** doubt에 HU 묶음 용어 정의일 수 있다고 적혀 있다. 기능의 범위로 보고 P06에 두었다. B17 HU 담당이 UNPACK 정의에 같은 문장을 넣으면 중복이 된다.
5. **P01 C 갈래의 REQ-MST-ORD-018.** 상태는 〈확인필요〉이고, 채택 여부는 Q-175의 안건이다. 두 가지를 모두 표시했다.
6. **입고 유형별 분기.** 지시대로 P02·P04의 1. 범위에 진입 조건 표로 쪼갰다. 「입고 오더 유형별로 경로를 구분한다」(REQ-MST-ORD-001)는 P02에만 두고, P04는 K16-F01.00 #2로 받았다.
7. **출력 위치.** 지침에 따라 `~/wt/chanki/정리본/` 아래에 썼다. CLAUDE.md의 「자기 worktree 밖은 읽기만」 규칙과 맞는지는 호출한 쪽에서 확인해 주기 바란다. git 명령은 쓰지 않았다.
