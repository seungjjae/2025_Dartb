### **1️⃣ 주요 개념**

- **복합 JOIN & GROUP BY + HAVING**:
    - `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`
    - `CROSS JOIN`, `SELF JOIN`
    - `GROUP BY`, `HAVING`

### JOIN
![image](https://github.com/user-attachments/assets/db258f1a-e5c3-469f-a863-9e918d9a9bd4)
두개 이상의 테이블을 사용해 쿼리를 작성하는 경우에 사용하는 쿼리간 결합 기능이다.
INNER, LEFT, RIGHT, FULL OUTER JOIN 이 존재한다. 모든 JOIN문법은 결합의 중심이 되는 컬럼이 존재하며 이를 ON을 사용해 묶어줘야한다.
- INNER JOIN : 두 테이블의 교집합되는 쿼리만을 추출하는 쿼리
- LEFT JOIN : A JOIN B 일 때 A를 기준으로 A의 연결쿼리의 모든부분이 출력되고 A에겐 없고 B에만 존재하는 행은 출력되지 않는다. A에만 있고 B에게 존재하지 않는 행은 결측치로 출력된다.
- RIGHT JOIN : LEFT JOIN을 오른쪽 기준으로 출력한 것
- FULL OUTER JOIN : A FULL OUTER JOIN B일때 서로 존재하는 모든행을 출력한다
- SELF JOIN : 같은 테이블내에서 JOIN을 사용해야하는 경우. EX) 사원의 담당 사수의 ID가 컬럼에 존재할 때 사수의 이름을 알고 싶을때 SELF JOIN을 사용해 쿼리를 작성한다.
- CROSS JOIN : 존재하는 모든 경우의수를 출력해주는 쿼리. 따라서 계산랑이 상당히 많아진다.

### GROUP BY, HAVING
- GROUP BY : 집계함수(SUM, MAX, MIN, AVG)를 사용할 때 어떤 컬럼을 기준으로 계산할 것인지 알기 위해서 사용하는 함수이다. AVG() OVER(), SUM() OVER()등을 사용해 일부 대체 가능하다.
- HAVING : GROUP BY를 사용한 후 조건을 걸어야 할 경우에 사용된다. 예를들어 요일별로 GROUP BY를 해서 매출을 SUM() 하였을 때 일정 이상의 매출이 있는 날이 궁금할 때 HAVING을 사용하면 GROUP BY 진행 이후에 조건을 걸어 쉽게 필터링
가능하다. 이외에도 쿼리 순서가 SELECT 이후인 점을 사용해 SELECT 에서 CASE WHEN으로 조건을 걸고 이에 대한 필터링으로도 사용가능하다.
---

### **2️⃣ 과제 안내**

각 개념에 대해 공식 문서를 참고하여 정리하고, 해당 개념을 활용하여 문제를 해결하세요.

### **✅ 복합 JOIN 학습 및 문제 풀이**

📖 **공식 문서 참고**: 🔗 [MySQL 공식 문서 - 조인](https://dev.mysql.com/doc/refman/8.0/en/join.html)

📝 **문제 풀이**: 🔗 [programmers - 저자별 카테고리 별 매출액 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/144856)
![image](https://github.com/user-attachments/assets/7c86fb3e-c31c-445b-9e38-47bb3d46107e)

```SQL
WITH 22data AS(
    SELECT 
    book_id,
    SUM(sales) AS sales
    FROM BOOK_SALES
    WHERE SALES_DATE BETWEEN '2022-01-01 00:00:00' AND '2022-01-31 23:59:59'
    GROUP BY book_id)
, info AS(
    SELECT 
    d.book_id,
    b.author_id,
    b.category,
    d.sales * b.price AS total_sales
    FROM 22data AS d JOIN BOOK AS b
    ON d.book_id = b.book_id)

SELECT 
i.author_id,
a.author_name,
i.category,
SUM(i.total_sales) AS total_sales
FROM info i JOIN AUTHOR a
ON i.author_id = a.author_id
GROUP BY author_name, category
ORDER BY author_id, category DESC
```

### **✅ GROUP BY + HAVING 학습 및 문제 풀이**

📖 **공식 문서 참고**

- 🔗 [MySQL 공식 문서 - GROUP BY](https://dev.mysql.com/doc/refman/8.0/en/group-by-handling.html) ([`ONLY_FULL_GROUP_BY`](https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html#sqlmode_only_full_group_by) mode 부분은 제외하셔도 됩니다.)
- 🔗 [MySQL 공식 문서 - HAVING](https://dev.mysql.com/doc/refman/8.0/en/select.html) (HAVING절에 관련된 부분만 학습해보세요~)

📝 **문제 풀이**: 🔗 [programmers - 언어별 개발자 분류하기](https://school.programmers.co.kr/learn/courses/30/lessons/276036) (도전)
![image](https://github.com/user-attachments/assets/f7907c94-ac5e-4090-982b-163daa559eea)
```SQL
WITH front AS(
    SELECT 
    CATEGORY,
    SUM(CODE) AS CODE
    FROM SKILLCODES
    WHERE CATEGORY = 'Front End'
    GROUP BY CATEGORY
    )

SELECT 
CASE WHEN SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'Python') 
AND SKILL_CODE & (SELECT CODE FROM front) THEN 'A'
WHEN SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'C#') THEN 'B'
WHEN SKILL_CODE & (SELECT CODE FROM front) THEN 'C'
ELSE Null END AS GRADE,
ID,
EMAIL
FROM DEVELOPERS
HAVING GRADE IS NOT NULL
ORDER BY GRADE, ID
```
& 연산자가 이진분류를 자동으로 처리해주어서 문제를 쉽게 풀 수 있었다.
---
