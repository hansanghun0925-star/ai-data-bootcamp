# 옷장패션 데이터 정제 보고서

## 1. 데이터 개요
- 행/열: 1,500 × 8
- 주요 컬럼: order_id, customer_age, category, channel, price, quantity, amount, return_amount

## 2. 진단 결과
- 결측: amount 3%, price 0.3%, ...
- 이상치(IQR): customer_age(999·0, 2건), quantity(200, 1건), amount(50,000,000, 1건)
- 의심되는 결측 유형: 결측 행 channel 분포 중 약 88%가 app으로 확인됨 -> MAR 의심 -> 그룹별 평균/중앙값 대체

## 3. 처리 결정과 근거
| 이슈 | 결정 | 근거 | 한계 |
| price 결측 | 카테고리별 중앙값 대체 | 관측치 5로 적음, 카테고리별 가격대 보존 | 표본이 적어 카테고리 통계 불안정 |
| amount 결측 | 채널별 중앙값 대체 | MAR 가설 부합 | 채널 외 다른 변수는 확인 안 함 |
| customer_age = 999, 0 | 중앙값 대체 | 물리적 불가능 값으로 판단 | 외부 인증 데이터 없음 |
| quantity = 200 | 중앙값 대체 | 일반 소비자 도메인 값 외 | 만약 도매 가능 고객이면 제거 손실 |
| amount = 50,000,000 | 유지 + amount_outlier 플래그  | 1건의 가능선 보존 | 도매면 별도 분석 필요 |


## 4. 처리 후 검증
- 결측 0건(설계상 NaN 유지가 필요한 컬럼 제외)
- customer_age 범위: 5 ~ 60 (정상)
- amount_outlier 플래그 145건 보존

## 5. 후속 권고
도매 가능 고객 식별을 위해 customer_id 단위 과거 이력 확보 필요

