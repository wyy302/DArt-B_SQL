# 문제 1 (ROOT 아이템 구하기)
```
어느 한 게임에서 사용되는 아이템들은 업그레이드가 가능합니다.
'ITEM_A'->'ITEM_B'와 같이 업그레이드가 가능할 때
'ITEM_A'를 'ITEM_B'의 PARENT 아이템,
PARENT 아이템이 없는 아이템을 ROOT 아이템이라고 합니다.
예를 들어 'ITEM_A'->'ITEM_B'->'ITEM_C' 와 같이 업그레이드가 가능한 아이템이 있다면
'ITEM_C'의 PARENT 아이템은 'ITEM_B'
'ITEM_B'의 PARENT 아이템은 'ITEM_A'
ROOT 아이템은 'ITEM_A'가 됩니다.
다음은 해당 게임에서 사용되는 아이템 정보를 담은 ITEM_INFO 테이블과 아이템 관계를 나타낸 ITEM_TREE 테이블입니다. ITEM_INFO 테이블은 다음과 같으며, ITEM_ID, ITEM_NAME, RARITY, PRICE는 각각 아이템 ID, 아이템 명, 아이템의 희귀도, 아이템의 가격을 나타냅니다.

ITEM_TREE 테이블은 다음과 같으며, ITEM_ID, PARENT_ITEM_ID는 각각 아이템 ID, PARENT 아이템의 ID를 나타냅니다.

단, 각 아이템들은 오직 하나의 PARENT 아이템 ID를 가지며, ROOT 아이템의 PARENT 아이템 ID는 NULL 입니다.
ROOT 아이템이 없는 경우는 존재하지 않습니다.

문제
---
ROOT 아이템을 찾아 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME)을 출력하는 SQL문을 작성해 주세요. 이때, 결과는 아이템 ID를 기준으로 오름차순 정렬해 주세요.
```
```SQL
SELECT A.ITEM_ID, A.ITEM_NAME
FROM ITEM_INFO AS A
JOIN ITEM_TREE AS B ON A.ITEM_ID = B.ITEM_ID
WHERE B.PARENT_ITEM_ID IS NULL
ORDER BY A.ITEM_ID
```
![4th_assignment_01](../4th_assignment/4th_assignment_image/4th_assignment_01.png)

---
# 문제 2 (노선별 평균 역 사이 거리 조회하기)
```
SUBWAY_DISTANCE 테이블은 서울지하철 2호선의 역 간 거리 정보를 담은 테이블입니다. SUBWAY_DISTANCE 테이블의 구조는 다음과 같으며 LINE, NO, ROUTE, STATION_NAME, D_BETWEEN_DIST, D_CUMULATIVE는 각각 호선, 순번, 노선, 역 이름, 역 사이 거리, 노선별 누계 거리를 의미합니다.

문제
---
SUBWAY_DISTANCE 테이블에서 노선별로 노선, 총 누계 거리, 평균 역 사이 거리를 노선별로 조회하는 SQL문을 작성해주세요.
총 누계거리는 테이블 내 존재하는 역들의 역 사이 거리의 총 합을 뜻합니다. 총 누계 거리와 평균 역 사이 거리의 컬럼명은 각각 TOTAL_DISTANCE, AVERAGE_DISTANCE로 해주시고, 총 누계거리는 소수 둘째자리에서, 평균 역 사이 거리는 소수 셋째 자리에서 반올림 한 뒤 단위(km)를 함께 출력해주세요.
결과는 총 누계 거리를 기준으로 내림차순 정렬해주세요.
```
```SQL
SELECT ROUTE, CONCAT(ROUND(SUM(D_BETWEEN_DIST),1), 'km') AS TOTAL_DISTANCE, CONCAT(ROUND(AVG(D_BETWEEN_DIST),2), 'km') AS AVERAGE_DISTANCE
FROM SUBWAY_DISTANCE
GROUP BY ROUTE
ORDER BY TOTAL_DISTANCE DESC
```

---
# 문제 3 (헤비 유저가 소유한 장소)
```
PLACES 테이블은 공간 임대 서비스에 등록된 공간의 정보를 담은 테이블입니다. PLACES 테이블의 구조는 다음과 같으며 ID, NAME, HOST_ID는 각각 공간의 아이디, 이름, 공간을 소유한 유저의 아이디를 나타냅니다. ID는 기본키입니다.

문제
---
이 서비스에서는 공간을 둘 이상 등록한 사람을 "헤비 유저"라고 부릅니다. 헤비 유저가 등록한 공간의 정보를 아이디 순으로 조회하는 SQL문을 작성해주세요.
```
```SQL
SELECT * FROM PLACES
WHERE HOST_ID IN (SELECT HOST_ID FROM PLACES
               GROUP BY HOST_ID
               HAVING COUNT(*) >= 2)
ORDER BY ID
```
![4th_assignment_02](../4th_assignment/4th_assignment_image/4th_assignment_02.png)