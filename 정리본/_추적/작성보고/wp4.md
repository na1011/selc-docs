# 작성 보고 — wp4

맡은 문서: P27 3층 사전·긴급 보충 · P28 3층 출고 구성 · P29 수작업 GTP 출고 · P30 직송 이적 · P31 출고완료 확정

## 1. 문서별 통계 (재료 기록 수 / 출처로 쓴 uid 수 / 반송 수)

| 문서 | 재료 기록 | 서로 다른 uid | 출처로 쓴 uid | 반송 |
|---|---|---|---|---|
| P27 3층 사전·긴급 보충 | 53 | 50 | 50 | 0 |
| P28 3층 출고 구성 | 145 | 142 | 142 | 0 |
| P29 수작업 GTP 출고 | 60 | 58 | 58 | 0 |
| P30 직송 이적 | 34 | 34 | 34 | 0 |
| P31 출고완료 확정 | 41 | 41 | 41 | 0 |

- 같은 uid의 다른 seq(예: BR-OUB-MIX-013 / #2, K33-OV02 / #5)는 각 seq의 내용을 문서 안 다른 줄에 썼다.
- 기록을 통째로 반송한 것은 없다. 일부 구절만 뺀 것은 4절에 있다.

## 2. 합친 중복 (대표 문장 요지 · uid 목록)

**P27**
- 새벽 1회 출고예정 → 당일·A타입 오더 추림 · BR-OUB-MIX-019, REQ-OUB-MIX-021, K32-OV03, REQ-OUB-MIX-014, REQ-MST-ALC-004, K32-F01.01, G214
- 사전 보충 수요 = 출고예정 − AS/RS FULL 팔레트 출고분 · G214, BR-OUB-MIX-014, REQ-OUB-MIX-015, K32-OV02, REQ-OUB-MIX-014
- 중소형 BOX 낱개 오더용 출고예정 기반 사전 보충 · G214, BR-EQP-BSH-007, BR-OUB-MIX-013, K32-OV01, REQ-OUB-MIX-013
- 3층도 부족하면 긴급 보충 · BR-OUB-MIX-015, K32-OV04, REQ-OUB-MIX-016, REQ-OUB-MIX-014
- 긴급 보충 BOX는 MIXED 직투입 없이 BOX Shuttle 적치 후 인출 · G217, BR-EQP-BSH-012, BR-OUB-MIX-017, REQ-OUB-MIX-019
- 2순위 = 긴급 보충 · G217, BR-OUB-MIX-022
- 적치율 임계치 → 사용자 알람 · K32-OV05, BR-OUB-MIX-018, REQ-OUB-MIX-020, K32-F01.03, BR-EQP-BSH-012, REQ-OUB-MIX-014
- 사전 보충 소요량에 여유 용량 상한 · BR-EQP-BSH-018, REQ-EQP-BSH-014(doubt 중복 지정), BR-OUB-MIX-018, REQ-OUB-MIX-014
- 수요 감소 시 Action 전송 전 취소·전송 후 완료·남는 BOX 미예약 재고 · BR-OUB-MIX-031, K32-EX05, REQ-OUB-MIX-028
- 출고예정 변경·취소 시 보충 BOX 유지 · BR-OUB-MIX-020, REQ-OUB-MIX-022

**P28**
- 1순위 BOX Shuttle → 2순위 AS/RS 팔레트 · BR-OUB-MIX-001, BR-OUB-MIX-008, BR-EQP-BSH-007, K32-OV01, REQ-OUB-MIX-001, REQ-MST-ALC-004
- BOX Shuttle은 입고월 FIFO → 셀 피킹 우선순위 · BR-MST-ALC-006, BR-INV-UNT-017, BR-EQP-BSH-014, REQ-EQP-BSH-011(doubt 중복 지정), REQ-MST-ALC-004
- 해체 BOX는 모두 BOX Shuttle 거쳐 MIXED · BR-OUB-MIX-021, BR-EQP-BSH-013, REQ-OUB-MIX-023, K32-OV05, REQ-EQP-BSH-010(doubt 중복 지정), G072
- 인도처 기준 스테이션 미사용 · BR-OUB-MIX-003, REQ-OUB-MIX-003
- 랩핑 제거 지시·완료 보고 · K24-F01.01, K25-F02.00, REQ-EQP-TRB-010, K32-F02.04
- 출고 구성 완료 팔레트 수직반송기 → AMR/유인지게차 → 출고 DOCK · REQ-EQP-VND-004 = REQ-EQP-MIX-005(문장 동일), BR-EQP-MIX-005, BR-EQP-VND-004, K24-F04.00·F06.00·F07.00, K32-F03.02
- 수직반송기·AMR 주고받기 · K20-IF07 = K24-IF06, K20-IF08 = K24-IF07, K20-IF09 = K24-IF08, K20-IF10 = K24-IF09
- 3층 도착 시 디팔레타이징 정보 · K24-IF01 = K32-IF07
- 출고 구성 지시 항목(인쇄물 항목 포함) · K24-IF04, K32-IF10, REQ-EQP-MIX-008, BR-EQP-MIX-009
- 출고 구성 완료 통보 · K24-IF05, K32-IF11 (K32 쪽이 「부족 BOX」를 더 적음)
- 디랩핑기 전달 항목 · K24-IF02, REQ-EQP-MIX-010, BR-EQP-MIX-011
- 부분 해체 피킹 실적 HMI → ECS · BR-EQP-ASRS-045, BR-OUB-MIX-028
- 출고 구성 후 잔량 → 경로 B BOX Shuttle · BR-EQP-BSH-002, BR-OUB-MIX-005, K32-OV06, BR-EQP-MIX-003
- 로딩 옵티마이저 수행 방식 · BR-OUB-MIX-006, REQ-OUB-MIX-009
- 마감 내 해소 불가 시 파워유저 선택 · REQ-OUB-MIX-007, K32-EX04
- 피킹지시 시점 부족 가능 · BR-OUB-MIX-004, K32-OV08
- 출고 구성 완료·잔량 통보 · K24-F02.02, K32-F03.01, K22-F02.02, K22-F06.04

**P29**
- GTP 판정 = W/T DEST BIN, 자체 판단 안 함 · BR-OUB-MAN-001, BR-OUB-MAN-026, REQ-OUB-MAN-001
- 3·4층 전달 완료 때 출고확정·원팔레트ID-n 실적 · BR-OUB-MAN-022, K33-OV02(#5), REQ-OUB-MAN-019, BR-EQP-ASRS-027, REQ-EQP-ASRS-030(doubt 중복 지정), REQ-EQP-ASRS-031
- 잔량 이동 W/T 생성 요청·확정 한 번에 · BR-OUB-MAN-023, BR-OUB-MAN-004, BR-OUB-MAN-020, REQ-OUB-MAN-004, REQ-OUB-MAN-016, REQ-OUB-MAN-020, REQ-INV-WT-005
- 출고확정 후 1층 이동 = EWMS W/T 수신 후 운반 · BR-OUB-MAN-025, REQ-OUB-MAN-022, REQ-OUB-MAN-010, REQ-OUB-MAN-011, K33-OV04

**P30**
- 유형이 다르면 팔레트갈이 → 3층 유도 · G088, G089(doubt 같은 규칙), BR-OUB-TRF-006, K34-OV02, REQ-OUB-TRF-006
- 출고 DOCK까지 Workflow 경로 분기 · BR-OUB-TRF-007, REQ-OUB-TRF-009
- 팔레트갈이 이동 Workflow 가변 · BR-OUB-TRF-011, REQ-OUB-TRF-011
- 3층 전달 완료 때 출고확정·원팔레트ID-n · BR-OUB-TRF-008, REQ-OUB-TRF-007, REQ-OUB-TRF-010, BR-EQP-ASRS-027, REQ-EQP-ASRS-030(doubt 중복 지정), REQ-EQP-ASRS-031

**P31**
- 확정 시점(수작업 인계 = 전달 완료, 나머지 = 설비 Flow 끝) · BR-OUB-CFM-010, K35-OV02, REQ-OUB-CFM-009
- 센서 / PDA / PC 일괄 · K35-F01.00·BR-OUB-CFM-001·REQ-OUB-CFM-001, K35-F02.00·BR-OUB-CFM-002·REQ-OUB-CFM-002, K35-F03.00·BR-OUB-CFM-003·REQ-OUB-CFM-003
- GTP·직송 이적 자동 확정 · G091, BR-OUB-CFM-009, REQ-OUB-CFM-008
- PARTIAL 수용 · BR-OUB-CFM-007, BR-OUB-MAN-021, REQ-OUB-MAN-018, REQ-OUB-CFM-006
- W/T 단위 취합 · BR-OUB-CFM-008, REQ-OUB-CFM-007
- 시리얼 출고 검수를 확정 화면에서 함께 · BR-MST-SN-014, K15-F04.03, BR-OUB-CFM-006, K35-F04.00, REQ-OUB-CFM-005
- 중복 확정·오스캔·확정 취소 · BR-OUB-CFM-012, K35-EX01, REQ-OUB-CFM-011
- 실적은 ORG DO 기준, SPLIT DO는 EWMS 내부 · K16-IF04, REQ-MST-ORD-015

## 3. 원문 불일치 (⚠ 표시한 곳 — 두 ID와 차이)

⚠ 쌍 19건(P27 7 · P28 3 · P29 5 · P30 4 · P31 0). 어느 쪽이 맞는지 고르지 않았다.

| # | 문서 | 한쪽 | 다른 쪽 | 차이 |
|---|---|---|---|---|
| 1 | P27 3-4 | REQ-OUB-MIX-014 ⑤ | BR-OUB-MIX-031 | 재계산 범위. ⑤는 「대상 범위는 새벽 생성분으로 고정, W/T 수신·설비 실적 수신 때 소요량만 갱신」. BR-031은 「출고예정 취소·수량 변경」도 소요량 갱신 계기로 적는다(출고예정 변경 때 재계산이 있는 것으로 읽힘). |
| 2 | P27 3-1 | REQ-OUB-MIX-014 ③ | REQ-MST-ALC-004 ③(→ ② 순서) | 사전 보충 보충원 팔레트 정렬. ③은 입고월 FIFO 우선 → 같은 입고월 안 잔량 최소·부분 팔레트. 초기 전략 값은 ② 순서(잔량 최소 → 부분 팔레트 → 입고월 FIFO). FIFO와 잔량 최소의 앞뒤가 반대다. |
| 3 | P27 3-2 | REQ-OUB-MIX-017 ②(= 014 ③ 기준) | BR-MST-ALC-009 | 긴급 보충 팔레트 정렬. 같은 차이(입고월 FIFO 우선 ↔ 잔량 최소 우선, 「FIFO와 충돌하면 잔량 최소 우선」). |
| 4 | P27 3-2 | REQ-OUB-MIX-005 | C02 「재할당」(출고 업무 용어 묶음) | 재할당의 두 뜻. REQ-005는 「재할당(= 긴급 보충): 전산 부족 감지 → 할당 우선순위에 따라 다음 순위 재고로 할당」. C02는 「설비가 출고 불가 사유를 전달하면 원 할당을 해제하고 같은 출고 조건의 대체 재고를 선정해 다시 할당」. 계기(전산 부족 ↔ 설비 불가 사유)와 대상(다음 순위 재고 ↔ 같은 조건의 대체 재고)이 다르다. |
| 5 | P27 7절 | K32-EX01 | C02 「재할당」 | 「재할당(긴급 보충) 우선순위」 — 4와 같은 뜻 차이. |
| 6 | P27 3-3 | BR-EQP-BSH-012, REQ-OUB-MIX-020 | BR-EQP-BSH-018, REQ-EQP-BSH-014, BR-OUB-MIX-018 | 적치율 임계치 도달 때 「보충을 막지 않는다」가 3층 보충 전체로 적혀 있다. 다른 쪽은 「긴급 보충은 막지 않고, 사전 보충은 여유 용량 상한 안에서만 만든다(소요량을 줄인다)」. 사전 보충에 적용되는 범위가 다르게 읽힌다. |
| 7 | P27 4절 | K32-IF01 | K31-IF01, C02 「출고예정」 | 출고예정 항목. K32는 화주가 있고, K31(1층)은 없으며, C02는 「화주는 지정되지 않는다(WCS가 맵핑)」. |
| 8 | P28 3-1 | REQ-MST-ALC-004 ② | REQ-OUB-MIX-011 ⑥ | 2순위 AS/RS 팔레트 정렬. ②는 잔량 최소 → 부분 팔레트 → 입고월 FIFO. ⑥은 입고월 FIFO 우선 → 같은 입고월 안 남는 BOX 수가 적은 팔레트. |
| 9 | P28 3-5 | BR-OUB-MIX-007, G058 | REQ-OUB-MIX-011 ③ | 전량 해체 재고 이력. BR-007·G058은 원팔레트 출고 + 3층 디팔레타이징 스테이징 셀 입고 → 적치 때 스테이징 셀 → BOX Shuttle 셀 이동. REQ-011 ③은 원팔레트 전량 출고 + BOX Shuttle 적치 BOX 입고(스테이징 셀 단계 없음). |
| 10 | P28 5-2 | K32-EX03(표 행 K32-EX06·08~12·14~16, REQ-OUB-MIX-006 첫 문장) | C02 「재할당」 | 부족 유형 표의 「재할당」은 긴급 보충으로 수행한다는 주석. 4와 같은 뜻 차이. S8(K32-EX13)과 REQ-OUB-MIX-006 둘째 문장은 「불가 사유 수신 → 대체 재고」라 C02 뜻과 같아 ⚠를 붙이지 않았다. K32-EX03 원문은 S8까지 포함한 표 전체에 주석을 단다. |
| 11 | P29 3-1 | BR-MST-ALC-009 | REQ-MST-ALC-004 ⑤ | GTP 팔레트 정렬. BR은 수량 일치 → 잔량 최소 → 부분 팔레트 → 입고월 FIFO. 초기 전략 값은 「부분 팔레트」 단계가 없다(생략일 수 있음). |
| 12 | P29 3-2 | REQ-OUB-MAN-003(확인필요) | REQ-OUB-MAN-002, K33-F05.05, K33-OV04 | WCS가 인도처별/FO별 팔레트 구성 지시를 제공한다 ↔ 분할·작업은 EWMS 영역, WCS PDA 분할 없음. |
| 13 | P29 3-3 | BR-OUB-MAN-009, REQ-OUB-MAN-006, BR-EQP-ASRS-018(#3), REQ-EQP-ASRS-031(#6) | REQ-EQP-ASRS-021(#2), BR-MST-LOC-006(#2), REQ-MST-LOC-004(#2) | GTP 재고 차감. 한쪽은 「예외 없이 전달 완료 때 출고확정으로 차감」, 다른 쪽은 「예외로 W/T 확정까지 WCS 재고 유지·관리」. BR-OUB-MAN-009 #4가 이 예외를 「삭제됐다」고 적어(8절 변경 이력) 다른 쪽이 옛 결정일 가능성이 있다. REQ-MST-LOC-004는 상태가 「확정」이다. |
| 14 | P29 3-4 | BR-OUB-MAN-012, REQ-OUB-MAN-020 | BR-INV-MVT-002, REQ-INV-MVT-004 ②(P41 재료) | GTP·직송 이적 잔량의 MMVT 전환. BR-OUB-MAN-012는 「AS/RS 재입고·MMVT 전환 안 함」, REQ-OUB-MAN-020은 DEST를 9013·RCV3 / 9014·RCV4로 둔다. BR-INV-MVT-002는 「자동화 재고를 수작업으로 옮길 때 MMVT로 위치 전환 후 출고」, REQ-INV-MVT-004 ②는 GTP·직송 이적 잔량을 「자동화 → 수작업(MMVT)」으로 분류한다. 상대 ID는 내 재료가 아니라 본문에 이름만 적었다. |
| 15 | P29 3-4 | REQ-OUB-MAN-020 | REQ-OUB-MAN-021, BR-OUB-MAN-024(P31) | 9013·RCV3 / 9014·RCV4를 REQ-020은 「출고대」로, 나머지는 「출고 수작업 셀」로 부르고 AS/RS 출고대 셀과 구분한다. |
| 16 | P30 3-3 | BR-OUB-TRF-008(+ REQ-OUB-TRF-007 ④, REQ-OUB-TRF-010) | K34-OV03 | 직송 이적 확정 시점. BR-008은 이적 전 3층 전달 완료 때 출고확정하고 이적은 WCS 재고 밖이다. K34-OV03은 「피킹 W/T는 이적 후 출고확정 때 확정」. |
| 17 | P30 3-2 | BR-OUB-TRF-002 | BR-OUB-TRF-008 | 배출 층. BR-002는 「이적 대상은 무조건 3층으로 배출」, BR-008은 「3층(4층) 전달 완료」. |
| 18 | P30 3-3 | BR-EQP-ASRS-018(#4), REQ-EQP-ASRS-031(#5) | REQ-EQP-ASRS-021(#3) | 직송 이적분 재고 차감. 13과 같은 차이. |
| 19 | P30 3-2 | BR-MST-ALC-009 | REQ-MST-ALC-004 ⑤ | 직송 이적 팔레트 정렬. 11과 같은 차이(「부분 팔레트」 단계 유무). |

**재할당 — C02 정의와의 차이 정리 (특기사항)**
- C02 재할당: 설비의 출고 불가 사유 → 원 할당 해제 → 같은 출고 조건의 대체 재고 선정 → 다시 할당.
- K32(3층)의 재할당: 전산 부족(W/T 할당·피킹지시 전·설비 실물 부족 보고) → 할당 우선순위의 다음 순위(AS/RS 팔레트 3층 배출·디팔레타이징 → BOX Shuttle) 재고로 할당 = 긴급 보충. 쓰인 곳: REQ-OUB-MIX-005, K32-EX01, K32-EX03과 그 표 행, REQ-OUB-MIX-006 첫 문장.
- 표시하지 않은 곳: K32-IF06 「AS/RS 팔레트 할당(2순위·재할당) 시」는 두 뜻 중 어느 쪽인지 원문만으로 알 수 없어 그대로 두었다. P30 REQ-OUB-TRF-008 ④ 「설비 출고 불가로 재할당」은 C02 뜻이다.

## 4. 반송·제외 (uid · 이유 · 보낼 곳)

통째로 반송한 기록은 없다. 문장 일부만 뺀 것:
- REQ-EQP-VND-004, K24-IF06 · 괄호 「수직반송기(설비사 미정)」을 뺐다. 설비의 설비사는 설비 묶음의 「설비사」 칸이 갖는다 · B25 운반 설비
- Q-207@K21 · 괄호 속 「재투입라인 설비사 현대무벡스」는 AS/RS 묶음 사실(doubt: BR-EQP-ASRS-045와 중복)이지만, 미결 질문의 기정 사항이라 P28 7절 미결 문장 안에 그대로 두었다 · (확인용) B19 AS/RS

## 5. 추가로 지운 기술 (uid · 지운 내용)

- REQ-OUB-MIX-005 · 「마감 역산 리드타임을 3층용 **파라미터로** 따로 관리」 → 「3층용으로 따로 관리」
- REQ-OUB-MIX-024 · 「같은 처리로 **구현해야 한다**」 → 「같은 처리로 한다」(doubt: 업무 결정이라 남김)

## 6. 비어 있는 칸 (문서 · 칸)

- P27 · 6. 기능·화면
- P29 · 7. 미결(원문 불일치는 3절 ⚠)
- P30 · 5. 예외, 6. 기능·화면, 7. 미결
- P27·P28·P29 외에는 8. 변경 이력 없음(P30·P31)

## 7. 다른 프로세스와 겹침 / 판단이 어려웠던 곳

**다른 프로세스와 겹침 (지우지 않고 자기 문서에 씀)**
- P27 ↔ P22 GANTRY 사전·긴급 보충: BR-OUB-MIX-030 ↔ BR-OUB-GNT-028(긴급 보충량 = 부족 − 진행 중 보충량), BR-OUB-MIX-031 ↔ BR-OUB-GNT-030(소요량 0 이하 → Action 전 취소·후 완료), REQ-OUB-MIX-028 ↔ REQ-OUB-GNT-034(수요 감소 처리). 1층·3층 공통 규칙이다.
- P27 ↔ P22: 재계산 계기는 층마다 다르게 적혀 있다(3층 REQ-OUB-MIX-014 ⑤ 「대상 범위 고정」 ↔ 1층 REQ-OUB-GNT-022 「출고예정 수신·변경 때도 재계산」). 층별 차이로 보고 ⚠는 3층 안의 모순(1번)에만 달았다.
- P27 ↔ P28: 해체 BOX의 MIXED 직투입 금지. P27은 긴급 보충 쪽(BR-OUB-MIX-017·REQ-OUB-MIX-019), P28은 2순위·사전·긴급 공통(BR-OUB-MIX-021·REQ-OUB-MIX-023)으로 썼다.
- P28 ↔ P23 GANTRY 출고: BR-OUB-MIX-029 #3(한 인도처 오더에 GANTRY·BOX 제품 → 층별 팔레트), BR-OUB-MIX-006·REQ-OUB-MIX-009(로딩 옵티마이저 = OUB-GNT와 같음), REQ-OUB-MIX-007(OUB-GNT 결정 동일 적용).
- P28 ↔ P34 재고 반영·보정: BR-OUB-MIX-007(관리단위 전환). doubt대로 BR-OUB-GNT-015와 같은 성격이라 공통 규칙 후보다.
- P28 ↔ P30: BR-EQP-MIX-010·REQ-EQP-MIX-009(3층 출고 구성 팔레트 경로 분기) ↔ BR-OUB-TRF-007·REQ-OUB-TRF-009(모든 출고 팔레트 경로 분기). P28에는 3층 팔레트분만 쓰고 → P30을 가리켰다.
- P28 ↔ P16 BOX Shuttle 적치·인출: 경로 B, BR-EQP-BSH-018 #3(임계치 때 BOX Shuttle 재고 우선 소진), BR-OUB-MIX-012(80% 임계치 알림은 P16으로 가리킴).
- P28 ↔ P15 TRASH BIN 공급: 결정로그 4절대로 3층 랩핑 제거 지시(K25-F02.00·K25-IF06·IF07·REQ-EQP-TRB-010·011)는 P28에 두었다.
- P29·P30 ↔ P31: 전달 완료 자동 출고확정(BR-OUB-MAN-022·BR-OUB-TRF-008 ↔ BR-OUB-CFM-009·REQ-OUB-CFM-008·G091). P29의 REQ-OUB-MAN-021(수작업 인계 재고 처리 순서) ↔ P31의 BR-OUB-MAN-024 #2(수작업 셀로 옮긴 뒤 확정).
- P29 ↔ P30: 잔량 이동 W/T(BR-OUB-MAN-023·REQ-OUB-MAN-020은 GTP·직송 이적 공통이라 P29에 쓰고, P30 BR-OUB-TRF-009는 P29를 가리킴)와 출고확정 후 1층 이동(BR-OUB-MAN-025·REQ-OUB-MAN-022 공통). doubt대로 「출고확정 후 1층 운반」을 별도 프로세스로 뺄지 검토 여지가 있다.
- P29 ↔ P35 W/T 연동: REQ-INV-WT-005 #2(잔량 이동 W/T 생성·확정 동시).

**판단이 어려웠던 곳**
- REQ-OUB-MIX-017(긴급 보충 방식)은 기록 전체가 「확인필요」다. 같은 뜻의 확정 기록과 합친 줄에도 규칙대로 [확인필요]를 붙이고, 출처에 「(확인필요)」를 적어 구분했다. 「긴급 보충은 사전 보충보다 우선」(REQ-OUB-MIX-014 ⑦은 확정)도 그래서 [확인필요]가 붙었다.
- BR-OUB-MIX-010(미결 Q-207)과 합친 부분 해체 줄에도 [미결 Q-207]이 붙었다. Q-207 자체는 「작업자 피킹 위치」만 묻는다.
- 사이즈 체크: G053·BR-OUB-MIX-027은 「한다」, G072·BR-OUB-MIX-010·REQ-OUB-MIX-012는 「기본으로 한다」. 모순으로 보지 않고 한 줄에 합치며 표현 차이를 괄호로 남겼다(G053 doubt: 옵션인지 확인).
- 잔량의 뜻: P29의 잔량(BR-OUB-MAN-023 「피킹 W/T에 할당되지 않은 수량」)은 「재고 수량」 묶음의 잔량 정의(「출고 구성이나 보충 후 남은 수량」)와 표현이 다르다. ⚠는 달지 않았고, P29 3-4에 이 프로세스에서의 잔량을 따로 적었다.
- G098 #3(「GANTRY 외에는 BOX Shuttle에 보관하거나 W/T를 생성해 이동」)은 특기사항대로 P28에 두고, W/T 생성 이동 쪽은 → P29·P30으로 가리켰다.
- PARTIAL 수용 규칙(BR-OUB-CFM-007·BR-OUB-MAN-021·REQ-OUB-MAN-018·REQ-OUB-CFM-006)은 모든 출고확정에 걸린다. doubt대로 「피킹 W/T는 PARTIAL 확정 가능」을 W/T 묶음(B03) 사실로 옮길지 2단계 판단이 필요하다. 지금은 P31에 두었다.
- REQ-OUB-TRF-008(팔레트갈이 판정 = Task 생성 시점)은 doubt의 §9-7 질문대로 업무로 남기고 「(잠정 §9-7)」을 붙였다.
- BR-OUB-MIX-012(「설비가 스스로 멈춘다」)는 설비 묶음 사실로 볼 여지가 있으나, 조건이 붙어 있어 P28 예외로 두었다.
- BR-OUB-MIX-021(MIXED 직투입 불가)은 doubt대로 물리 제약이 아니라 운영 결정(대기 시간)이라 프로세스 규칙으로 두었다.
- K32 §7 부족 유형 표의 S2(인도처·호기 불일치 — 해당 없음)는 재료에 없어 쓰지 않았다(BR-OUB-MIX-003 「스테이션 할당 안 함」은 3-1에 있음).
