# [프로세스] AS/RS Bypass 이송

> 키워드: EQP-ASRS · 원문 기준일 2026-11-02
> **기대는 묶음** (사실을 알아야 이해되는 것): 「AS/RS」, 「W/T」, 「EWMS BIN 코드」
> **이름만 쓰는 묶음**: 「시스템」, 「HU」, 「에러·사유코드」, 「팔레트」, 「입고 오더」
> **이어지는 프로세스**: 앞 → P02 일반입고(1층 입고대 투입), EWMS 이동 W/T 지시(→ P35 W/T 연동) / 뒤 → P11 3층 출고대 도착 팔레트 처리

## 1. 범위
EWMS가 지시해 화물을 AS/RS를 통해 다른 층으로 옮기는 이동이다. 투입 때 시작해 도착 층에서 W/T를 확정할 때 끝난다. 〔REQ-EQP-ASRS-031〕
어떤 업무(OEM·쉽백 등)의 Bypass인지는 구분하지 않는다. 〔REQ-EQP-ASRS-032〕

## 2. 흐름

Bypass 경로(기본 둘과 경우의 수 하나)는 「AS/RS」 묶음 3.7에 있다. 경로마다 투입·도착·W/T가 다르다. 〔BR-EQP-ASRS-040, BR-EQP-ASRS-055, BR-EQP-ASRS-056〕

| 경로 | 투입 | 도착 | W/T | 출처 |
|---|---|---|---|---|
| 1층 → 3층 | 1층 입고대 | 3층 출고대 | 적치 W/T | 〔BR-EQP-ASRS-040, BR-EQP-ASRS-031, BR-EQP-ASRS-034, REQ-EQP-ASRS-031〕 |
| 4층 → 3층 | 4층 입고대 | 3층 출고대 | 이동 W/T | 〔BR-EQP-ASRS-040, BR-EQP-ASRS-031, BR-EQP-ASRS-034, REQ-EQP-ASRS-031〕 |
| 1층 → 4층 (경우의 수) | 1층 입고대 | 4층 | 적치 W/T(DEST 9014·RCV4) | 〔BR-EQP-ASRS-040, BR-EQP-ASRS-055, BR-EQP-ASRS-056〕 |

Bypass 팔레트가 나오는 3층 출고대는 「AS/RS」 묶음 3.2 「3층 배출」에 있다. 〔BR-EQP-ASRS-015〕

### 2-1. 1층 → 3층
1. (EWMS→WCS) 적치 W/T를 처음부터 DEST BIN = 3층 출고대(9013·RCV3), 행선지 = Bypass로 준다. 〔BR-EQP-ASRS-053, REQ-EQP-ASRS-041〕
2. (WCS) 1층 입고대 투입 시 적치 W/T를 HU 내역 조회로 연결한다. 〔REQ-EQP-ASRS-041〕
3. (ECS) AS/RS를 거쳐 3층 출고대로 배출한다. 〔REQ-EQP-ASRS-041, BR-EQP-ASRS-040〕
4. (WCS→EWMS) 3층 출고대 도착 시 적치 W/T를 EWMS가 지정한 DEST BIN(9013·RCV3) 그대로 확정한다. 〔REQ-EQP-ASRS-041, BR-EQP-ASRS-053, K21-IF06, REQ-EQP-ASRS-031〕

### 2-2. 4층 → 3층
1. (EWMS→WCS) 이동 W/T로 Bypass를 지시한다. 〔K21-IF04, REQ-EQP-ASRS-032〕
2. (ECS→WCS) 팔레트가 4층 입고대에 투입되면 목적지를 문의한다. 〔BR-EQP-ASRS-054, REQ-EQP-ASRS-049, K21-IF07〕
3. (WCS→ECS) 팔레트 ID로 수신한 이동 W/T를 조회해 목적지를 3층 출고대로 응답한다. 〔BR-EQP-ASRS-054, REQ-EQP-ASRS-049, K21-IF07〕
4. (ECS) AS/RS를 거쳐 3층 출고대로 옮긴다. 〔REQ-EQP-ASRS-032〕
5. (WCS→EWMS) 3층 출고대 도착 시 이동 W/T를 확정한다. 〔REQ-EQP-ASRS-049, REQ-EQP-ASRS-032, K21-IF05, REQ-EQP-ASRS-031〕

### 2-3. 1층 → 4층 (경우의 수)
1. (EWMS→WCS) 적치 W/T의 DEST가 4층 수작업(9014·RCV4)이다. 〔BR-EQP-ASRS-056, REQ-EQP-ASRS-050〕
2. (WCS→ECS) 1층 입고대에서 이 팔레트를 받으면 목적지를 4층으로 응답한다. 〔REQ-EQP-ASRS-050〕
3. (ECS) AS/RS를 거쳐 4층에 배출한다. 〔BR-EQP-ASRS-056, REQ-EQP-ASRS-050〕
4. (WCS→EWMS) 4층 도착 시 적치 W/T를 EWMS가 지정한 DEST(9014·RCV4) 그대로 확정한다. 1층 → 3층과 같은 방식이다. 〔REQ-EQP-ASRS-050〕

## 3. 조건·규칙

| 조건 | 처리 | 출처 |
|---|---|---|
| Bypass의 W/T 종류 | 경로로 구분한다. 1층에서 출발하면 적치 W/T, 4층에서 출발하면 이동 W/T다 | 〔BR-EQP-ASRS-031, BR-EQP-ASRS-034, BR-EQP-ASRS-040〕 |
| 1층 출발 Bypass의 DEST BIN | EWMS가 준 DEST BIN을 바꾸지 않고 그대로 확정한다 | 〔BR-EQP-ASRS-053, REQ-EQP-ASRS-041, REQ-EQP-ASRS-050〕 |
| 업무 구분 | 어떤 업무(OEM·쉽백 등)의 Bypass인지는 구분하지 않는다 | 〔REQ-EQP-ASRS-032〕 |

## 4. 주고받는 정보

| 송신→수신 | 시점 | 업무 항목 | 출처 |
|---|---|---|---|
| EWMS→WCS | 4층 → 3층 Bypass 지시 시 | 이동 W/T — W/T 번호, 팔레트 ID, 제품, 수량, 출발 BIN·도착 BIN | 〔K21-IF04, REQ-EQP-ASRS-032〕 |
| ECS→WCS | 4층 → 3층 Bypass 4층 입고대 투입 시 | 목적지 문의 — 팔레트 ID | 〔K21-IF07〕 |
| WCS→ECS | 위 문의에 대한 응답 | 목적지 응답 — 3층 출고대(이동 W/T 조회 결과) | 〔K21-IF07〕 |
| WCS→EWMS | 4층 → 3층 Bypass 도착 시 | 이동 W/T 확정 — W/T 번호, 팔레트 ID, 도착 위치 | 〔K21-IF05〕 |
| WCS→EWMS | 1층 → 3층 Bypass 3층 출고대 도착 시 | 적치 W/T 확정 — W/T 번호, 팔레트 ID(DEST BIN은 EWMS 지정 9013·RCV3 그대로) | 〔K21-IF06〕 |

## 5. 예외
- 4층 입고대 투입 팔레트에 이동 W/T가 없으면 Reject(사유 = W/T 없음)로 응답한다. 〔REQ-EQP-ASRS-049〕
- 3층 출고대 도착 이후는 → P11 3층 출고대 도착 팔레트 처리.

## 7. 미결
(이 프로세스에 걸린 미결 번호는 재료에 없음)
