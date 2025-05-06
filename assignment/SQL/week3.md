### **1️⃣ 주요 개념**

- **CASE문 & 논리 연산자 활용**:
    - `CASE WHEN`
    - `IF()`, `IFNULL()`, `NULLIF()`
    - `COALESCE()`
- 해당 문법의 개념과 사용 시 주의할 점들을 정리하여 깃허브에 정리해 주세요.  

CASE WHEN : 조건문으로 자주 사용된다. 특히 다중조건문에서 CASE WHEN (조건1) THEN 'A' WHEN (조건2) THEN 'B' ELSE 'C' END 와 같은 형태로 파이썬에서의 ELIF와 같은 형태로 사용가능하다

IF : CASE WHEN과 비슷하게 조건문이지만 CASE WHEN 보다는 단순조건일 때 사용된다. IF(조건, 'A', 'B') AS COLUMN1 과 같은 형태로 A 혹은 B 와 같은 경우에 자주 사용된다. CASE WHEN 처럼 다중 조건문에도 사용가능하긴 하지만
쿼리가 복잡해져 지양된다.  

INFULL : NULL값 대체에 사용된다. IFNULL(COLUMN1,'B') 란 뜻은 컬럼 COLUMN1에서의 결측치를 모두 'B'로 대체하라는 뜻이다. 결측치가 아닌 경우에는 기존 값이 그대로 유지된다.  

NULLIF : 기존 데이터를 NULL값으로 만드는데 사용된다. NULLIF(COLUMN1, 10) 이란 뜻은 COLUMN1에 존재하는 10의값들을 모두 NULL로 변환시킨다. 조건에 해당하지 않는 절들은 원래값들을 유지한다.  

---

### **2️⃣ 과제 안내**

각 개념에 대해 공식 문서를 참고하여 정리하고, 해당 개념을 활용하여 문제를 해결하세요.

### **✅ CASE문 & 논리 연산자 학습 및 문제 풀이**

📖 **공식 문서 참고**:

- 🔗 [MySQL 공식 문서 - 흐름제어함수](https://dev.mysql.com/doc/refman/8.4/en/flow-control-functions.html)
- 🔗 [MySQL 공식 문서 - 비교, 논리연산자](https://dev.mysql.com/doc/refman/8.4/en/comparison-operators.html)

📝 **문제 풀이**:

- 🔗 [HackerRank - 삼각형 종류 분류하기](https://www.hackerrank.com/challenges/what-type-of-triangle/problem) `CASE문`
![image](https://github.com/user-attachments/assets/dc969160-f375-4b19-ad6c-23f16ce51a50)
```SQL
SELECT 
    CASE   
        WHEN A + B <= C THEN 'Not A Triangle'
        WHEN A = B AND B = C THEN 'Equilateral'
        WHEN A = B OR B = C OR A = C THEN 'Isosceles'
        ELSE 'Scalene' END
FROM TRIANGLES;
```
- 🔗 [LeetCode - find-customer-referee](https://leetcode.com/problems/find-customer-referee/description/) `IS NULL`
![image](https://github.com/user-attachments/assets/b2024808-d3a1-4ea3-828b-0ef944e5947c)
```SQL
SELECT name
FROM Customer
WHERE referee_id !=2 OR referee_id IS NULL
```
