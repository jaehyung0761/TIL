# Tody I Learned SQL

> **Note**: SQL을 사용하여 데이터 다루기


## Table of Contents

  1. [SQL기본문법](#SQL기본문법)
  1. [SQL고급문법](#SQL고급문법)

  
### SQL기본문법
  - 1.1 **SELECT문**
    - **`SELECT - FROM`**
      >  employees 데이터베이스 사용하기
      ```python
      USE employees;
      ```
      >  존재하는 테이블 목록 보기
      ```python
      SHOW TABLES;
      ```
    
      > 사원 테이블에서 사원들의 이름만 가져와 보자.
      ```python
      SELECT first_name, last_name
      FROM   employees;
      ```
    
      > 사원 테이블에서 사원들의 이름만 가져와 보자.
      ```python
      SELECT first_name, last_name
      FROM   employees;
      ```
    - **`SELECT - FROM - WHERE`**
      > 사원 테이블에서 이름이 'Elvis'인 사람의 정보를 가져와 보자
      ```python
      SELECT *
      FROM   employees
      WHERE  first_name = 'Elvis';
      ```
    - **`AND OR`**
      > 사원중에서 사번이 20000이상이고, 20100이하인 사람들의 이름과 성별을 출력해 보자.
      ```python
      SELECT first_name, last_name, gender
      FROM   employees
      WHERE  emp_no >= 20000
      AND    emp_no <= 20100;
      ```
      > 사원들 중에서 성이 'Genin'이거나 'Facello'인 사원의 정보를 출력해보자.
      ```python
      SELECT *
      FROM   employees
      WHERE  last_name = 'Genin'
      OR     last_name = 'Facello';
      ```
    - **`BETWEEN AND`**
      > 사원중에서 사번이 20000이상이고, 20100이하인 사람들의 이름과 성별을 출력해 보자.
      ```python
      SELECT first_name, last_name, gender
      FROM employees
      WHERE emp_no BETWEEN 20000 AND 20100;
      ```
    - **`날짜함수`**
      > 날짜 출력함수 사용하기
      ```python
      DATE_FORMAT(<field 명>, '%Y') : 연도를 출력
      DATE_FORMAT(<field 명>, '%M') : 월을 출력
      DATE_FORMAT(<field 명>, '%D') : 일자를 출력
      DATE_FORMAT(<field 명>, '%Y%M%D') : 20191217
      DATE_FORMAT(<field 명>, '%Y-%M-%D') : 2019-12-17
      DATE_FORMAT(<field 명>, '%Y%M%D %H-%i-%p') : 20191217 14-12 pm (24)
      DATE_FORMAT(<field 명>, '%Y%M%D %h-%i-%p') : 20191217 14-12 pm (12)
      ```
    
      > 사원중에 입사한 일자가 '19850101' ~ '19860101'인 사원들의 정보를 출력해보자.
      ```python
      SELECT *
      FROM   employees
      WHERE  date_format(hire_date, '%Y%m%d') BETWEEN 19850101 AND 19860101
      ORDER BY hire_date desc           # 정렬;
      ```
    - **`IN함수`**
      ```python
      <field명> IN ('A값','B값')
      ```
      > 사원들 중에서 성이 'Genin'이거나 'Facello'인 사원의 정보를 출력해보자.
      ```
      SELECT *
      FROM   employees
      WHERE  last_name IN ('Genin','Facello');
      ```
    - **`LIKE함수`**
      ```python
      <field명> LIKE 's%' : s로 시작하는 데이터 
      <field명> LIKE '%s' : s로 끝나는 데이터 
      <field명> LIKE '%s%' : s를 중간에 포함하는 데이터 
      <field명> LIKE '__s' : 3글자인데 2글자를 모르면 _로 채우고 조회한다. 
      ```
      > 사원중에서 직무가 'Engineer'로 끝나는 사람의 사번을 출력해보자.
      ```
      SELECT emp_no
      FROM   titles
      WHERE  title LIKE '%Engineer';
      ```
    - **`NULL함수`**
      > 사원중에서 salary가 NULL이 아닌 사원의 정보를 가져와 보자.
      ```python
      SELECT *
      FROM   salaries
      WHERE  salary IS NOT NULL;
      ```
    - **`count함수`**
      > 사원번호가 d004 또는 d002인 사원의 수를 출력해보자.
      ```python
      SELECT COUNT(dept_no) AS 사원수
      FROM   dept_emp
      WHERE  dept_no IN ('d004','d002')
      ```
    - **`GROUP BY`**
      > 1999년 입사자를 월별로 몇명이 입사했는지 출력해보자.
      ```python
      SELECT   MONTH(hire_date),COUNT(emp_no)
      FROM     employees
      WHERE    YEAR(hire_date) = 1999
      GROUP BY MONTH(hire_date)
      ```
    - **`AVG 함수`**
      > 영업이나 마케팅 업무를 하는 사원을 대상으로 직무별 평균급여를 계산해보자.
      ```python
      SELECT    job_id,
                avg(salary)
      FROM      hr_employees
      WHERE     job_id LIKE 'SA%'
      OR        job_id LIKE 'MK%'
      GROUP BY  job_id
      ```
    - **`MOD 함수`**
      > 짝수해에 입사한 사원의 정보를 얻어보자.
      ```python
      SELECT * 
      FROM   hr_employees
      WHERE  MOD(DATE_FORMAT(hire_date,'%Y'),2)=0
      ```
    - **`SELECT CASE`**
      > 짝수해에 입사한 사원과 홀수해에 입사한 사원의 정보를 얻어보자.
      ```python
      SELECT CASE mod(YEAR(hire_date),2)
            WHEN 0 THEN '짝수년도'
            WHEN 1 THEN '홀수년도'
           END AS '년도',
           COUNT(employee_id) AS 사원수
      FROM   hr_employees
      GROUP BY mod(YEAR(hire_date),2)
      ```
    - **`ROUND 함수`**
      > 숫자를 지정한 자리수로 반올림하여 변환하는 함수
      ```python
      ROUND(123.32429382,2)
      ```
    - **`CONCAT 함수`**
      > 사원의 이름을 출력해보자.
      ```python
      SELECT CONCAT(first_name,' ',last_name)
      FROM   hr_employees
      WHERE  employee_id = 105
      ```
    - **`SYSDATE 함수`**
      > 현재 근무하는 사원의 재직기간을 나타내보시오.
      ```python

      ```
