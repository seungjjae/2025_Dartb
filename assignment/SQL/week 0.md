## **📌 Week 0: 서브쿼리 & CTE(공통 테이블 표현식)**

### **1️⃣ 주요 개념**

- **서브쿼리**: `EXISTS`, `NOT EXISTS`, `IN`, `ANY`, `ALL`
- **CTE (공통 테이블 표현식)**: `WITH`
    - 🚨 **`WITH RECURSIVE`는 4주차에 다룰 예정이므로, 이번 주차에서는 `WITH` 중심으로 학습**해 주세요.
    - 해당 문법의 개념과 사용 시 주의할 점들을 정리하여 깃허브에 정리해 주세요.

- EXISTS => 보통 WHERE에서 사용된다(SELECT, HAVING에서도 사용은 가능). 조건을 만족하는 경우가 서브쿼리에 하나라도 존재하면 '참'을 반환한다. 이를 사용하여 조건을 걸 수 있다.
- NOT EXISTS => subquery에서의 조건을 가지지 않은 데이터만을 반환하는 방식으로 사용가능하다.
- IN => WHERE절에서 조건을 걸때 자주 사용된다. WHERE절에서 WHERE column_name IN(1000, 1700)로 사용하면 column_name의 값이 1000,1700인 경우만 필터링한다.  혹은 IN()에 subquery를 넣어 조건을 만족하는 데이터만 반환하는 방식으로 사용가능하다.
- ANY => WHERE절에서 column_name > ANY(subqery) 와 같은 방식으로 사용된다. 이 경우 column_name이 subqery에서 select한 어느 한 값보다 크면 필터링 되는 방식이다.
- ALL => ANY보다 조건이 까다로운 경우이다. 모든 경우에 subquery보다 값이 커야한다.
---

### **2️⃣ 과제 안내**

각 개념에 대해 공식 문서를 참고하여 정리하고, 해당 개념을 활용하여 문제를 해결하세요.

### **✅ 서브쿼리 학습 및 문제 풀이**

📖 **공식 문서 참고**: 🔗 [MySQL 공식 문서 - 서브쿼리](https://dev.mysql.com/doc/refman/8.0/en/subqueries.html)

- **[공식 문서 정리 범위]**
    - 15.2.15. Subqueries
    - 15.2.15.2. Comparisons Using Subqueries
    - 15.2.15.3. Subqueries with ANY, IN or SOME
    - 15.2.15.4. Subqueries with ALL
    - 15.2.15.6. Subqueries with EXISTS or NOT EXISTS
    - 15.2.15.10. Subquery Errors

📝 **문제 풀이**:

[문제 1]
- 🔗 [Solvesql - 많이 주문한 테이블](https://solvesql.com/problems/find-tables-with-high-bill/) (맛보기 문제 → 너무 쉽죠? ㅎㅎ)
![image](https://github.com/user-attachments/assets/120b62bd-fcd4-4621-9056-740fb35ef9b6)

```
SELECT *
FROM tips
WHERE total_bill > (SELECT AVG(total_bill) FROM tips)

```




[문제 2]
- 🔗 [Solvesql - 레스토랑의 대목](https://solvesql.com/problems/high-season-of-restaurant/)
![image](https://github.com/user-attachments/assets/336d841f-c8d4-4145-93ba-093de571f6b0)
```
SELECT 
  t.total_bill,
  t.tip,
  t.sex,
  t.smoker,
  t.day,
  t.time,
  t.size
FROM tips t
INNER JOIN(
SELECT 
  day,
  SUM(total_bill) AS mean
FROM tips
GROUP BY day
HAVING mean >= 1500) m
ON t.day = m.day

```


### **✅ CTE (`WITH`) 학습 및 문제 풀이**

📖 **공식 문서 참고**: 🔗 [MySQL 공식 문서 - WITH](https://dev.mysql.com/doc/refman/8.0/en/with.html) 

* `WITH RECURSIVE`에 대한 내용은 4주차에 공부합니다. 해당 링크에서 `WITH` 에 해당하는 부분만 정리해보세요.


[문제 3]
📝 **문제 풀이**:  🔗 [programmers - 식품분류별 가장 비싼 식품의 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131116) 

→ 서브쿼리와 with를 모두 활용하여 각각 풀이해보고 각 방식의 장단점을 정리해보세요.

---


```
## WITH문 사용한 경우
WITH high_price AS(
    SELECT 
    CATEGORY,
    MAX(PRICE) AS MAX_PRICE
    FROM FOOD_PRODUCT
    WHERE CATEGORY IN ('과자', '국', '김치', '식용유')
    GROUP BY CATEGORY)

SELECT 
h.CATEGORY,
h.MAX_PRICE,
f.PRODUCT_NAME
FROM FOOD_PRODUCT f JOIN high_price h
ON h.MAX_PRICE = f.PRICE AND h.CATEGORY = f.CATEGORY
ORDER BY MAX_PRICE DESC

```


```
## SUBQUERY 사용한 경우
SELECT
CATEGORY,
PRICE AS MAX_PRICE,
PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE (CATEGORY, PRICE) IN (
    SELECT 
    CATEGORY,
    MAX(PRICE) AS MAX_PRICE
    FROM FOOD_PRODUCT
    WHERE CATEGORY IN ('과자', '국', '김치', '식용유')
    GROUP BY CATEGORY)
ORDER BY MAX_PRICE DESC
```


개인적으로 WITH문을 사용했을 때 좀 더 쉽게 쿼리를 작성할 수 있는 것 같다. WITH문은 보통 JOIN을 하는식으로 사용하기 때문에 작성하는 사람 입장에서 쉽다. 하지만 WITH문은 본인이 빌드업을 하며 '이런식으로 사용하면 되겠다' 라는 생각이 들어 남들은 이해하기 어려울 수 있다고 생각한다.  
하지만 SUBQUERY가 처음 볼 때 더 직관적으로 이해할 수 있다고 생각한다. 수학에서의 괄호기호처럼 먼저 SUBQUERY를 보고 이후에 그 조건이 어떻게 활용되는지 해석하기 때문에 보는사람 입장에서는 SUBQUERY가 더 이해하기 쉽다고 생각한다.




### **3️⃣ 공식 문서 활용 팁**

MySQL 공식 문서는 영어로 제공되지만, **크롬 브라우저에서 공식 문서를 열고 `이 페이지 번역하기`에서 `한국어`를 선택하면 번역된 버전으로 확인할 수 있습니다.** 다만, 번역본은 **문맥이 어색한 부분**이 종종 있으니 **영어 원문과 한국어 번역본을 왔다 갔다 하며 확인**하거나, **교육팀장의 정리 예시를 참고하셔도** 괜찮습니다.

---

### **4️⃣ 과제 제출 방식**

1. **공식 문서를 참고하여 해당 개념을 정리**
2. **정리한 개념과 문제 풀이 내용을 깃허브에 업로드**
3. **과제 업로드 시트에 깃허브 링크 첨부**


```
