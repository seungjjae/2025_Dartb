## **📌 Week 4: CTE, GROUP_CONCAT()**

---

### **1️⃣ 주요 개념**

- **CTE, GROUP_CONCAT()**:
    - `WITH RECURSIVE`
    - `GROUP_CONCAT()`

### WITH RECURSIVE
- 자기 자신을 반복적으로 호출하는 쿼리를 만들 때 사용
- WITH RECURSIVE를 사용해 WITH문을 작성하고 이후 반드시 UNION을 사용해 기존 테이블과 RECURSIV테이블을 합쳐야한다.
- SUB QUERY에서 반드시 반복문을 사용해야한다.

### GROUP_CONCAT()
- 집계함수로서 GROUP BY로 묶인 문자열을 ,(쉼표)로 연결하여 새로운 문자열로 만드어준다.

---

### **2️⃣ 과제 안내**

각 개념에 대해 공식 문서를 참고하여 정리하고, 해당 개념을 활용하여 문제를 해결하세요.

### **✅ GROUP_CONCAT() 학습 및 문제 풀이**

📖 **공식 문서 참고**

- 🔗 [MySQL 공식 문서 - GROUP_CONCAT()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_group-concat)
    
    (1주차에 OVER절을 이용해서 윈도우함수처럼 사용한 부분을 학습하실 때 활용한 공식문서와 동일합니다. GROUP_CONCAT()부분을 중심으로 공부해주세요.)
    

📝 **문제 풀이**:

- 🔗 [programmers - 우유와 요거트가 담긴 장바구니](https://school.programmers.co.kr/learn/courses/30/lessons/62284) **`GROUP_CONCAT()`**
    
    ```jsx
    조건:
    - WITH 구문을 사용해 가상의 테이블을 먼저 만들고,
    - GROUP_CONCAT() 을 활용하여 상품명을 하나의 문자열로 합친 후,
    - 그 문자열을 활용해 Milk와 Yogurt가 모두 존재하는 장바구니만 필터링해야 합니다.
    ```
    
    → 성능 상 더 좋은 쿼리를 짜는 방법들이 있지만, 이번 문제는 **GROUP_CONCAT()**을 활용하여 데이터를 가공하는 연습을 목표로 합니다.
    
```SQL
WITH MENU AS(SELECT 
CART_ID,
GROUP_CONCAT(NAME) AS TOTAL
FROM CART_PRODUCTS
GROUP BY CART_ID)

SELECT CART_ID
FROM MENU
WHERE TOTAL LIKE '%Milk%' AND TOTAL LIKE '%Yogurt%'
```
![image](https://github.com/user-attachments/assets/ff1d6bd1-5987-420e-a6a5-13f476c877ba)

    

### **✅ CTE(`WITH RECURSIVE`) 학습 및 문제 풀이**

📖 **공식 문서 참고**

- 🔗 [MySQL 공식 문서 - WITH RECURSIVE](https://dev.mysql.com/doc/refman/8.0/en/with.html)
    
    (0주차에 학습한 공식문서와 동일합니다. `WITH RECURSIVE` 를 위주로 학습해주세요.)
    

📝 **문제 풀이**:

- 🔗 [programmers - 입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413) **`WITH RECURSIVE`**

```SQL
WITH RECURSIVE dummy1 AS(
    SELECT 0 AS HOUR
UNION ALL
    SELECT HOUR+1
    FROM dummy1
    WHERE HOUR<23)

SELECT
d.HOUR,
COUNT(DISTINCT ANIMAL_ID) AS COUNT
FROM dummy1 d LEFT JOIN ANIMAL_OUTS a
ON d.HOUR = HOUR(a.DATETIME)
GROUP BY d.HOUR
```
재귀문에서 COUNT를 사용해서 한번에 끝내볼려고 했지만 그렇게 했을 시 0,1,2와 같이 COUNT가 0 인 값들은 컬럼에서 사라져 표현되지 않았다.  
따라서 반복문을 사용해 0~23까지 나타내는 컬럼을 만들고 거기에 기존 TABLE을 JOIN 했어야 했다.
이때 COUNT(*)를 사용하면 HOUR = 0 의 비이었는 행도 카운트하므로 COUNT(DISTINCT ANIMAL_ID)를 사용해 값이 할당되어있는 행만을 따로 카운트했다.

---

### **3️⃣ 과제 제출 방식**

1. **공식 문서를 참고하여 해당 개념을 정리**
2. **정리한 개념과 문제 풀이 내용을 깃허브에 업로드**
3. **과제 업로드 시트에 깃허브 링크 첨부**
