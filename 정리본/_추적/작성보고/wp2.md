# 작성 보고 — wp2

담당: P08~P17 (프로세스 10개). 쓴 파일은 `정리본/프로세스/` 아래 P08_ASRS_적치.md, P09_ASRS_인출.md, P10_ASRS_Bypass_이송.md, P11_3층_출고대_도착_팔레트_처리.md, P12_ASRS_용량_부족_배출.md, P13_설비_출고_불가_재할당.md, P14_공팔레트_공급.md, P15_TRASH_BIN_공급.md, P16_BOX_Shuttle_적치·인출.md, P17_설비_접점_인계.md다.

## 1. 문서별 통계 (재료 기록 수 / 출처로 쓴 uid 수 / 반송 수)

같은 uid가 #seq만 다르게 여러 번 나온 경우(예: REQ-EQP-TRB-009 #3·#4)는 uid 하나로 센다.

| 문서 | 재료 기록 | 고유 uid | 출처로 쓴 uid | 반송·이동 |
|---|---|---|---|---|
| P08 AS/RS 적치 | 43 | 42 | 42 (+ P15에서 옮겨 온 BR-EQP-ASRS-048) | 0 (부분 반송 1) |
| P09 AS/RS 인출 | 18 | 18 | 18 | 0 |
| P10 AS/RS Bypass 이송 | 17 | 17 | 17 | 0 |
| P11 3층 출고대 도착 팔레트 처리 | 18 | 18 | 18 | 0 |
| P12 AS/RS 용량 부족 배출 | 20 | 20 | 20 | 0 |
| P13 설비 출고 불가 재할당 | 11 | 10 | 10 | 0 |
| P14 공팔레트 공급 | 87 | 78 | 78 | 0 (부분 반송 2) |
| P15 TRASH BIN 공급 | 39 | 37 | 33 | 4 (P08로 이동 3, 반송 1) |
| P16 BOX Shuttle 적치·인출 | 22 | 21 | 21 | 0 |
| P17 설비 접점 인계 | 19 | 16 | 16 | 0 |

## 2. 합친 중복 (대표 문장 요지 · uid 목록)

- P08 팔레트 유형 불일치 → Reject: G132 · BR-EQP-ASRS-025 · REQ-EQP-ASRS-027 (REQ-EQP-ASRS-025 ③은 조건이 넓어서 따로 한 줄로 씀)
- P08 AS/RS 투입 DEST = 자동화구역(0050·WCS1), 다른 프로세스로 가는 재고만 9011·9013: BR-MST-LOC-022 · REQ-MST-LOC-021
- P08 적치셀 통보 → 이동 이력 → 적치 W/T 확정·가용: K21-F01.04 · K21-F01.05 · BR-INV-WT-004 · BR-EQP-ASRS-019 · REQ-EQP-ASRS-022 · REQ-EQP-ASRS-002(확인필요 → 합친 줄에 [확인필요])
- P08 3층 입고대 판별(목적지 문의·응답): BR-EQP-ASRS-046 · REQ-EQP-ASRS-044
- P08 적치 W/T 없는 재투입 Reject, W/T 처리 없음: BR-EQP-ASRS-047 · BR-EQP-ASRS-050 · REQ-EQP-ASRS-046 · REQ-EQP-ASRS-044
- P08 투입 순서는 작업자 임의: BR-EQP-ASRS-049 · REQ-EQP-ASRS-046
- P08 적치 실패·셀 이상: BR-EQP-ASRS-059 · K21-EX03 · REQ-EQP-ASRS-052
- P08 TRASH BIN 3층 입고대 투입, W/T 없이 자체 Work·Task: REQ-EQP-TRB-017 · BR-EQP-ASRS-048 · REQ-EQP-ASRS-044
- P08 미등록 TRASH BIN Reject: K25-EX03 · REQ-EQP-TRB-009
- P09 3층 배출라인 지정: REQ-EQP-ASRS-017 · BR-EQP-ASRS-015
- P09 3·4층 배출 시 재고 차감: BR-EQP-ASRS-018 · REQ-EQP-ASRS-021 (GTP·직송 이적 부분은 ⚠로 나눔)
- P09 W/T가 있었던 건 배출 완료 시 확정: BR-EQP-ASRS-027 · REQ-EQP-ASRS-030
- P09 피킹 작업지시·완료 통보: K21-F02.01 · REQ-EQP-ASRS-003 / K21-F02.03 · REQ-EQP-ASRS-004 (확인필요 → [확인필요])
- P10 Bypass 경로와 W/T 종류: BR-EQP-ASRS-031 · BR-EQP-ASRS-034 · BR-EQP-ASRS-040 · BR-EQP-ASRS-055 · REQ-EQP-ASRS-031
- P10 1층 → 3층 적치 W/T 그대로 확정: BR-EQP-ASRS-053 · REQ-EQP-ASRS-041 · K21-IF06
- P10 4층 → 3층 목적지 문의·이동 W/T 조회: BR-EQP-ASRS-054 · REQ-EQP-ASRS-049 · K21-IF07
- P10 1층 → 4층: BR-EQP-ASRS-056 · REQ-EQP-ASRS-050
- P11 도착 출고대(포트 코드) 보고: BR-EQP-ASRS-051 · K21-IF13 · REQ-EQP-ASRS-047 · REQ-EQP-ASRS-042
- P11 배출 사유 제공·상황판 표시: BR-EQP-ASRS-007 · K21-IF12 · REQ-EQP-ASRS-011
- P11 유인지게차 이동, WCS 작업지시 없음: BR-EQP-ASRS-011 · REQ-EQP-ASRS-014
- P11 PDA 판단 기능: BR-EQP-ASRS-013 · REQ-EQP-ASRS-015 / 행선지 표시: BR-EQP-ASRS-016 · REQ-EQP-ASRS-018 / 작업 추적: BR-EQP-ASRS-017 · REQ-EQP-ASRS-020
- P12 용량 부족 → 3층 출고대 배출: BR-EQP-ASRS-006 · REQ-EQP-ASRS-009 · BR-EQP-ASRS-015 · K21-EX02 · BR-INB-PLT-011 · K26-OV02
- P12 적치 W/T 기준 DEST BIN 바꿔 확정: BR-EQP-ASRS-032 · G081 · BR-INB-PLT-011 · K21-EX02 · REQ-EQP-ASRS-031 · REQ-EQP-ASRS-033 (4층 DEST 부분은 ⚠)
- P12 재투입 시 EWMS가 W/T 새로 생성: BR-EQP-ASRS-035 · G081 · REQ-EQP-ASRS-034
- P13 출고 불가 사유 전달 → 대체 재고 재할당: K21-F02.04 · K21-F02.05 · REQ-EQP-ASRS-006 · REQ-EQP-ASRS-007 · BR-EQP-ASRS-005 · BR-EQP-VND-010 · K21-EX01
- P14 매거진 우선 사용, 추가 필요 시만 요청: K12-F62.01 · K12-F62.02 · BR-MST-EQP-015 · BR-MST-EQP-016 · G075 · G133 · REQ-MST-EQP-022 · K12-OV03 · K12-F56.00 · K12-F57.00 · K12-F58.00
- P14 제품과 같은 경로·설비 접점: K12-F62.04 · G126 · BR-MST-EQP-010 · REQ-MST-EQP-014
- P14 AS/RS 공팔레트 묶음 피킹 지시: K12-F62.04 · BR-MST-EQP-016 · REQ-MST-EQP-010 · REQ-MST-EQP-024 · G133 · G134
- P14 공급라인 행선지: REQ-MST-EQP-035 · BR-MST-EQP-029
- P14 공급 불가 리턴: BR-MST-EQP-018 · K12-EX01 · REQ-MST-EQP-025 · K12-IF03
- P14 매거진 배출 요청(대표·적재 팔레트 ID): BR-MST-EQP-014 · BR-MST-EQP-019 · BR-MST-EQP-020 · K12-F64.01 · REQ-MST-EQP-017 · REQ-MST-EQP-026 · BR-HU-VAL-011 · BR-HU-VAL-004
- P14 묶음 구성 등록·이력: K12-F64.02 · REQ-MST-EQP-026 · REQ-HU-VAL-008 · REQ-MST-EQP-032
- P14 매거진 배출 묶음 AS/RS 적치 지시: K12-F65.00 · BR-MST-EQP-020 · BR-HU-VAL-011 · REQ-MST-EQP-011 · REQ-MST-EQP-018 · REQ-MST-EQP-026
- P14 1층 매거진 배출 → 1층 입고대(Workflow): BR-MST-EQP-024 · G075 · BR-HU-VAL-008 ([미결 Q-202]가 가장 약한 상태)
- P14 3층 MIXED 1816 → 3층 출고대 → 유인지게차: REQ-MST-EQP-031 · BR-MST-EQP-025 · BR-MST-EQP-027 ([미결 Q-216])
- P14 매거진 배출 요청 항목: K12-IF06 · K29-IF09 / 적치 작업지시 항목: K12-IF07 · K29-IF10
- P14 적용 설비 같은 방식: BR-MST-EQP-017 · REQ-MST-EQP-024 / 3층 입고 팔레타이징로봇 유형 지정 요청: BR-MST-EQP-003 · REQ-MST-EQP-003 · K12-F58.00 · G028
- P15 만재 육안 판단 → 버튼 → 보충 요청: BR-EQP-TRB-016 · BR-EQP-TRB-017 · K25-F04.01 · BR-EQP-TRB-005 · K25-OV03 · REQ-EQP-TRB-013 · REQ-EQP-TRB-014 · REQ-EQP-TRB-022 · G153
- P15 랩핑 제거 지시와 별개: BR-EQP-TRB-015 · K25-F04.00 · K25-F04.01 · K25-OV03 · REQ-EQP-TRB-012 · G153
- P15 빈 통 선정·예약·출고 지시: K25-F04.02 · REQ-EQP-TRB-002 · REQ-EQP-TRB-005 · REQ-EQP-TRB-013
- P15 도착 알림·요청 종료·위치 반영: BR-EQP-TRB-025 · K25-IF03 · K25-F04.04 · REQ-EQP-TRB-009 · REQ-EQP-TRB-022
- P15 중복 요청 「진행 중」: REQ-EQP-TRB-005 · K25-EX02
- P16 용량 부족 시 설비 자체 멈춤·80% 알림: BR-EQP-BSH-009 · BR-EQP-BSH-010 · REQ-EQP-BSH-007 · K22-EX01
- P16 공간 확보(Q-131): BR-EQP-BSH-011 · BR-EQP-BSH-018 · K22-EX01
- P17 접점에서 송신 완료 → 수신 지시: BR-EQP-VND-002 · REQ-EQP-VND-003
- P17 수직반송기 신호 항목: BR-EQP-VND-017 · G033 · REQ-EQP-VND-009
- P17 접점 장애(Q-055): K20-EX01 · K24-EX01 · Q-055

## 3. 원문 불일치 (⚠ 표시한 곳 — 두 ID와 차이)

1. **P12 — 용량 부족 배출을 「Bypass」로 적음.** REQ-EQP-ASRS-025 ④는 「해당 유형 호기가 모두 고장·만재이면 용량 부족과 같이 3층 Bypass한다」고 적는다. K26-OV02 · REQ-EQP-ASRS-039(그리고 재료 밖 BR-EQP-ASRS-039)는 용량 부족 배출을 Reject의 한 유형으로 보고 Bypass와 구분한다.
2. **P12 — 용량 부족 배출 목적지.** BR-EQP-ASRS-006 · REQ-EQP-ASRS-009 · BR-EQP-ASRS-015는 3층 출고대로만(「항상」) 배출한다고 적는다. BR-EQP-ASRS-030 · REQ-EQP-ASRS-031 · REQ-EQP-ASRS-033은 DEST BIN을 「3층 출고대(9013·RCV3) / 4층 입고장(9014·RCV4)」으로 바꿔 확정한다고 적어 4층 배출을 전제한다.
3. **P08 — 1층 입고대 Reject 목적지 (새로 찾음, 2번과 같은 꼴).** BR-EQP-ASRS-015는 1층 입고대 Reject가 항상 3층 출고대로 나온다고 하고, REQ-EQP-ASRS-027은 DEST BIN을 9013·RCV3로 둔다. 반면 BR-EQP-ASRS-030 · REQ-EQP-ASRS-031은 「3·4층으로 토출」하고 4층 입고장(9014·RCV4) DEST도 적는다.
4. **P09 — GTP·직송 이적분의 재고 차감 시점.** 원문 BR-EQP-ASRS-018은 GTP 출고분·직송 이적분도 예외 없이 3·4층 전달 완료 시 출고확정으로 차감한다고 적는다. 원문 REQ-EQP-ASRS-021은 같은 두 갈래를 예외로 두고 W/T 확정까지 WCS 재고를 유지한다고 적는다.
   - 재료 판의 두 문장에는 이 부분이 빠져 있었다. 담당 특기사항에 따라 원문 문구를 ⚠ 두 줄로 옮겼다. 처리 주인은 P29·P30이다.
   - 재료 K21-IF08(W/T 확정에서 GTP·직송 이적 제외)과도 함께 확인해야 한다.
5. **P16 — BOX Shuttle 적치에 W/T가 따르나.** REQ-EQP-BSH-002는 모든 적치에 적치 W/T 확정이 따르는 것처럼 적는다. 재료 밖의 REQ-EQP-BSH-009(3층 사전 보충 BOX는 자동화구역 안 이동이라 W/T 없음)와 Q-016(경로 B W/T 필요 여부 [추가 협의])은 W/T가 없거나 미정이라고 한다. P16에는 REQ-EQP-BSH-002만 출처로 달고, 상대 ID는 설명 줄에 이름으로만 적었다.
6. **P15 — 빈 TRASH BIN이 없을 때 (새로 찾음).** BR-EQP-TRB-022 · REQ-EQP-TRB-018은 처리를 정했다(공급 불가 리턴·알람·대기·자동 재처리). K25-EX01은 같은 상황을 「처리 미정(Q-139)」이라 한다. 원문 Q-139의 본문은 TRASH BIN 형태·규격 안건이라 번호도 맞지 않아 보인다.

## 4. 반송·제외 (uid · 이유 · 보낼 곳)

**반송**
- K26-EX01 · 디팔레타이징 전 DeWrapping 체적정보 요청(Q-107)은 랩핑 제거 쪽 일이고 TRASH BIN 공급과 별개다(BR-EQP-TRB-015) · P28 3층 출고 구성

**담당 문서끼리 옮김 (P15 → P08)**
빈 TRASH BIN을 3층 입고대로 AS/RS에 넣는 일은 공급이 아니라 3층 입고대 적치의 한 갈래라서 P08 2-3에 썼다.
- BR-EQP-ASRS-042 (#4)
- BR-EQP-ASRS-048 (#2)
- REQ-EQP-ASRS-044 (#3)

**부분 반송** (uid는 문서에 남고, 문장 일부만 뺐다)
- K12-F62.05 · 「매거진 재고는 WCS 재고로 관리하지 않는다」는 늘 참인 대상 사실이다 · B16 팔레트
- BR-HU-VAL-004 · 「일반입고와 동일하게 검증한다」는 검증 규칙이다(P14에는 → P05 연결만 둠) · P05 HU 확인·생성
- BR-EQP-ASRS-029 · 「1층 입고대 Reject는 Bypass와 구분한다」는 용어 Reject의 정의다 · B05 에러·사유코드

## 5. 추가로 지운 기술 (uid · 지운 내용)
- 없음. 재료에 기술 단어(메소드·트랜잭션·테이블·API·invn_id·wh_id·파라미터·커밋)는 남아 있지 않았다.
- REQ-EQP-ASRS-019 ②「행선지는 Task의 Workflow 목적지에서 가져온다」는 데이터 출처라 기술일 수 있다(§9-7). 1단계 메모대로 남겼다(P11 3절).

## 6. 비어 있는 칸 (묶음 · 칸)
프로세스 문서라 묶음 칸 대신, 재료가 없어 비워 둔 절을 적는다.
- P08·P09·P10·P11·P12·P13 §7 미결: 재료에 미결 번호 없음
- P12 §5 예외: 재료에 따로 없음 (용량 부족 배출 자체가 투입의 예외다)
- P09·P10·P13·P17 §6 기능·화면, §8 변경 이력: 재료에 없어 절을 두지 않음

## 7. 다른 프로세스와 겹침 / 판단이 어려웠던 곳

**다른 프로세스와 겹침**
- P08 5-4 적치 실패·셀 이상(REQ-EQP-ASRS-052) ↔ P44 설비 에러·Reject 처리(REQ-SYS-ERR-006 ②「BOX Shuttle 예외와 같은 방식」) · P39 재고대사
- P08 투입~적치 재고 변화(BR-EQP-ASRS-019, REQ-EQP-ASRS-022) ↔ P02 일반입고(BR-INB-PLT-014, REQ-INB-PLT-014)
- P08 Reject W/T(BR-EQP-ASRS-030, REQ-EQP-ASRS-031) ↔ P02 일반입고(BR-INB-PLT-017·027·028·030·033)
- P09 ↔ P24 FULL 팔레트 출고: K21-F02 흐름은 원래 FULL 팔레트 출고 절에 있었다. P24는 P09를 가리키고 단계를 다시 적지 않아야 한다.
- P09 재고 차감·W/T 확정 ↔ P29 수작업 GTP 출고(BR-OUB-MAN-022, REQ-OUB-MAN-008), P30 직송 이적(BR-OUB-TRF-008, REQ-OUB-TRF-003)
- P12 ↔ P02 일반입고: BR-INB-PLT-011이 재료에 같이 왔다. 일반입고 쪽에도 용량 부족 예외가 남아 있으면 같은 규칙이다.
- P13 ↔ P23 GANTRY 출고(OUB-GNT S8, REQ-OUB-GNT-015 선택지), P28 3층 출고 구성(REQ-OUB-MIX-006 BOX Shuttle 출고 불가), P32 재고 할당(REQ-MST-ALC-004 ④)
- P14 묶음 구성 등록·이력(REQ-HU-VAL-008·009, BR-HU-VAL-004·008·011) ↔ P05 HU 확인·생성
- P15 진행 중 요청 도착 지연 알람(REQ-EQP-TRB-005 ③) ↔ P43 팔레트 작업 상태 추적 · P46 단말 화면·알림(REQ-SYS-UI-016)
- P16 스테이징 셀 → BOX Shuttle 셀 이동(REQ-INV-UNT-028) ↔ P27 3층 사전·긴급 보충 · P28 3층 출고 구성
- P17 수직반송기 신호 처리(REQ-EQP-VND-008·010, BR-EQP-VND-020) ↔ P43 팔레트 작업 상태 추적(REQ-SYS-ARC-015·016)

**판단이 어려웠던 곳**
- **P09를 공통 단계로 썼다.** 흐름은 「피킹셀 지시 → 인출 → 완료 통보 → 3·4층 배출 시 재고 차감·W/T 반영」까지만 둔다. 무엇을 왜 꺼내는지는 부르는 쪽에 맡겼다.
  - 3층 배출라인 지정 표는 여러 작업이 같이 쓰므로 P09에 두었다.
- **확인필요 기록을 합친 줄**(REQ-EQP-ASRS-002·003·004·005, REQ-EQP-BSH-001·003): 규칙대로 합친 단계에 [확인필요]를 붙였다. 같은 단계의 원문 흐름(K21-F01.x·F02.x, K22-F06.x)은 상태가 없었다. 그래서 확정된 흐름이 [확인필요]로 보일 수 있다. 출처 괄호에 어느 uid가 확인필요인지 적어 두었다.
- **REQ-SYS-ERR-006**은 상태가 확정이다. 다만 「예시이며 Q-211 때 확정」이라 끝줄에만 [미결 Q-211]을 붙였다(P16 5절).
- **BR-MST-EQP-004** 「오류 처리 방식 미정」은 Q번호가 없어 상태 표시 없이 「원문에서 정하지 않았다」로 적었다.
- **REQ-EQP-VND-003**은 범위를 「P-01~P-14」로 적는다. 확정 접점 P-16·P-17이 빠진 것으로 보인다. P17에는 원문 범위 표기를 그대로 남겼다. 접점 목록을 가진 B18에서 확인해야 한다.
- **REQ-EQP-VND-005** 「WCS는 수직반송기를 다른 설비사 설비로 본다」는 B18·B25의 사실일 수 있다. 인계 흐름의 이유라서 P17에 남겼다.
- **REQ-EQP-MIX-004**(확인필요) 「접점 구간 인계 중 BOX 위치·상태 관리」는 3층 내부 구간이 접점에서 빠진(P-03·P-04·P-15 폐기) 뒤라 뜻이 불분명하다. [확인필요]로 P17에 두었다.
- **REQ-MST-LOC-021**은 예시로 「3·4층 배출」을 들면서 BIN은 9011·9013만 적는다. 4층 입고장 9014가 빠져 있다(P08 3절에 그대로 둠). 3번 불일치와 함께 확인해야 한다.
- **BR-EQP-ASRS-015 (#4)** 「Bypass 팔레트는 3층 출고대로 나온다」는 1층 → 4층 경로(BR-EQP-ASRS-040 ③·056)와 맞지 않아 보인다. 원문 맥락은 3층 배출라인의 용도 목록이라 불일치로 표시하지는 않았다(P10).
- **REQ-MST-EQP-029 (#2)** 「1층 공팔레트 묶음 배출·공급 라인 셀을 공급·배출 작업의 출발·도착 위치로 쓴다」는 Q-202의 「매거진 만재 묶음은 이 라인 미사용」과 「배출」의 뜻이 겹친다. 그대로 두고 확인 대상으로 남긴다.
- **BR-EQP-BSH-009**(BOX Shuttle 용량 부족 시 설비 자체 멈춤)는 B20 BOX Shuttle의 「운영 상태」 칸 사실일 수도 있다. 담당 특기사항대로 P16 용량 관리 절에 두었다.
- **작업 위치**: 지침대로 `~/wt/chanki/정리본/` 아래에 썼다. 저장소 CLAUDE.md는 자기 worktree 밖 파일을 고치지 않게 하므로, 이 위치가 맞는지 통합 담당이 확인해야 한다.
