![image](https://github.com/user-attachments/assets/528c49d0-20da-4414-98df-237534e7bef9)## **📌 Week 1: 윈도우 함수 (Window Functions)**

---

### **1️⃣ 주요 개념**

- **윈도우 함수 (Window Functions)**:
    - `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
    - `LAG()`, `LEAD()`
    - `SUM() OVER()`, `AVG() OVER()`
    - `PARTITION BY`, `ORDER BY` 등
- 윈도우 함수
    - 각 행의 값을 유지하면서 특정 범위 내에서 연산을 수행하는 함수
```SQL
SELECT
  class,
  id,
  MAX(score)
FROM Student
GROUP BY class
```

예를 들어 위와 같은 코드는 class 내에서 최고의 score를 가진 값을 반환하는 것이 목적일 것이다. 하지만 위와 같이 group by에 포함되지 않은 id가 select 들어갈 경우 id는 class의 가장 상단의 것으로 배치된다. 
이러한 것을 방지하고자 윈도우 함수를 사용해서 범위 내에서만 연산을 수행하는 것이다.

---

### 윈도우 함수 정리
- OVER() : 윈도우 함수 뒤에 와야되는 함수로 이것이 와야 앞의 문법이 윈도우 함수인 것으로 인식된다. OVER의 ()사이엔 목적에 따라 PARTITION BY, ORDER BY등이 올 수 있다. 이를 통해 그룹을 나누고 정렬 기준을 정렬할 수 있다.
- RANK() : 동일한 값이면 같은 값을 부여하고 다음 순위는 건너띄는 순위 부여 함수 => 1,2,2,4
- DENSE_RANK() : 동일한 값이면 같은 값을 부여하고 다음 순위는 건너띄지 않는 순위 부여 함수 => 1,2,2,3
- ROW_NUMBER() : 동일한 값이라고 고유한 번호를 부여하는 순위 부여 함수 => 1,2,3,4
- LAG(column, n) : 이전 n번째 값을 가져오는 함수. 가져올 값이 없으면 NULL 부여
- LEAD(column, n) : 이후 n번째 값을 가져오는 함수. 가져올 값이 없으면 NULL 부여
- SUM() OVER() : 누적 합계. 그 행에다만 조건을 거는 식으로 자주 사용됨
- AVG() OVER() : 누적 평균. 그 행에다만 조건을 거는 식으로 자주 사용됨

### **✅ 윈도우 함수 (Window Functions) 학습 및 문제 풀이**

📖 **공식 문서 정리하기**

- **[공식 문서 정리 범위]: 아래의 순서대로 내용을 확인하시면 정리에 도움이 되실 겁니다.** 반드시 **모든 내용을 정리할 필요는 없으며** 본인에게 도움이 될 만한 새로 배운 내용들을 정리해주시면 충분합니다.
    - 🔗 [MySQL 공식 문서 - 14.20.2. Window Function Concepts and Syntax](https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html)
    - 🔗 [MySQL 공식 문서 - 14.20.1. Window Function Descriptions](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html)
    - (🔗 [MySQL 공식 문서 - 14.20.4. Named Windows](https://dev.mysql.com/doc/refman/8.0/en/window-functions-named-windows.html) ) - OPTIONAL
    - 🔗 [MySQL 공식 문서 - 14.19.1. Aggregate Function Descriptions](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html)
        
        → over_clause를 이용하여 집계함수를 윈도우함수처럼 사용하는 부분만 공부해보세요.
        

📝 **문제 풀이**:

- 🔗 [LeetCode - Rank Scores](https://leetcode.com/problems/rank-scores/description/) `DENSE_RANK()`
![image](https://github.com/user-attachments/assets/da97ed63-5560-43c7-814c-35f25b3fbc3b)
```SQL
SELECT 
    score,
    DENSE_RANK() OVER(ORDER BY score DESC) AS 'rank'
FROM Scores
```
- 🔗 [Solvesql - 다음날도 서울숲의 미세먼지 농도는 나쁨 😢](https://solvesql.com/problems/bad-finedust-measure/) `LEAD()`
![image](https://github.com/user-attachments/assets/2f5493f2-d8fb-4ac3-b6d9-5e4bab7265d9)
```SQL
SELECT *
FROM(
  SELECT 
  measured_at AS today,
  LEAD(measured_at,1) OVER() AS next_day,
  pm10 AS pm10,
  LEAD(pm10,1) OVER() AS next_pm10
  FROM measurements)
WHERE next_pm10 > pm10
```
- 🔗 [programmers - 그룹별 조건에 맞는 식당 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131124) (도전!!)
![image](https://github.com/user-attachments/assets/c2502298-ebb3-427a-826f-e0677b32540d)
```SQL
WITH query1 AS(
    SELECT 
        MEMBER_ID,
        COUNT(*) OVER (PARTITION BY MEMBER_ID) AS review_count, 
        REVIEW_TEXT,
        REVIEW_DATE
    FROM REST_REVIEW
),
query2 AS(
    SELECT 
        *, 
        DENSE_RANK() OVER(ORDER BY review_count DESC) AS ranked
    FROM query1)

SELECT 
MEMBER_PROFILE.MEMBER_NAME,
query2.REVIEW_TEXT,
DATE_FORMAT(query2.REVIEW_DATE, '%Y-%m-%d') AS REVIEW_DATE
FROM query2 JOIN MEMBER_PROFILE
ON query2.MEMBER_ID = MEMBER_PROFILE.MEMBER_ID
WHERE ranked = 1
ORDER BY REVIEW_DATE, REVIEW_TEXT
```

    → 사실 문제만 어떻게든 풀려면 쉽게 풀 수 있었을 것이지만 윈도우 함수를 사용하려 하니 꽤 힘들었다... 윈도우 함수에 대한 연습이 더 필요해 보인다.
