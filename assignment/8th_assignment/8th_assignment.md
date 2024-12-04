# 1. PIVOT과 UNPIVOT

## 학습 요약
- 가시성이 확보된 데이터 형태와 집계함수를 사용할 수 있는 데이터 형태는 구분되어 있음
- 이 두 가지 데이터 형태를 변경하기 위해 사용하는 함수

---

- 칼럼을 기준으로 여러 개의 행으로 나뉜 데이터를 행과 열을 전환해 테이블을 재구성하여 보기 편하도록 만드는 것

- PIVOT : 행을 열로 바꾼다, 지정된 칼럼의 각 행 속성값들이 새로운 칼럼 헤더가 되고 이에 맞게 전체 속성값들이 재배치된다.

- UNPIVOT : PIVOT과 반대로 열을 행으로 바꾼다. 칼럼 헤더들이 한 칼럼의 각 행 속성값이 되고 이에 맞게 전체 속성값들이 재배치된다.

---

![8th_assignment_01](../8th_assignment/8th_assignment_image/8th_assignment_01.png)

- 실행 결과 데이터 형태는 GROUP BY 와 같은 집계하수는 사용하기는 편하지만 가시성이 부족함

![8th_assignment_02](../8th_assignment/8th_assignment_image/8th_assignment_02.png)

- 이와 같이 PIVOT을 활용하면 가시성이 확보된 데이터 형태로 변환됨

- 행을 열로 바꾸지만 전치 행렬과는 다른 개념

![8th_assignment_03](../8th_assignment/8th_assignment_image/8th_assignment_03.png)

- UNPIVOT은 정확히 반대 개념

---

## 문제 풀이

```
Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.
Note: Print NULL when there are no more names corresponding to an occupation.

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

Explanation
The first column is an alphabetically ordered list of Doctor names.
The second column is an alphabetically ordered list of Professor names.
The third column is an alphabetically ordered list of Singer names.
The fourth column is an alphabetically ordered list of Actor names.
The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with NULL values.
```

```
문제

OCCUPATIONS 테이블에서 Occupation 열을 피벗(Pivot)하여, 각 Name이 알파벳 순으로 정렬된 상태로 해당하는 직업(Occupation) 아래에 표시되도록 합니다. 출력 열의 헤더는 각각 Doctor, Professor, Singer, Actor가 되어야 합니다.

조건

특정 직업에 해당하는 이름이 없는 경우, 빈 셀 대신 “NULL”을 출력합니다.
Occupation 열은 “Doctor”, “Professor”, “Singer”, “Actor” 중 하나만 포함합니다.

출력 형식 설명

첫 번째 열(Doctor)에는 알파벳 순으로 정렬된 Doctor의 이름 목록이 표시됩니다.
두 번째 열(Professor)에는 알파벳 순으로 정렬된 Professor의 이름 목록이 표시됩니다.
세 번째 열(Singer)에는 알파벳 순으로 정렬된 Singer의 이름 목록이 표시됩니다.
네 번째 열(Actor)에는 알파벳 순으로 정렬된 Actor의 이름 목록이 표시됩니다.
어떤 열이라도 직업별 최대 이름 수보다 적은 경우, 해당 열의 나머지 행은 “NULL”로 채웁니다.
```

# 2. 성능 최적화 기법

## 칼럼 요약

쿼리 최적화 팁 6가지

0. 인덱싱
- 인덱싱 : 데이터베이스에서 원하는 정보를 찾기 위한 "지름길"을 만드는 것

- 데이터 검색 속도가 크게 향상되고, 데이터를 효율적으로 관리하고 활용하는 데 큰 도움이 됨

1. 좌변을 연산하지 않을 것

```SQL
SELECT * FROM sales WHERE YEAR(date) = 2021;
```

- 직관적이고 이해하기 쉬운 코드이지만 데이터베이스 관점에서는 효율적이지 않음

- 한 줄 한 줄 일일이 YEAR(date) 연산을 수행하고, 그 결과가 2021인지 확인하는 작업이 계속되므로 데이터가 많을수록 작업량이 엄청남

```SQL
SELECT * FROM sales 
WHERE date >= '2021-01-01' AND date <= '2021-12-31';
```

- 