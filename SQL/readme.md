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
      FROM titles
      WHERE title LIKE '%Engineer';
      ```
    - **`NULL함수`**
      > 사원중에서 salary가 NULL이 아닌 사원의 정보를 가져와 보자.
      ```python
      SELECT *
      FROM   salaries
      WHERE  salary IS NOT NULL;
      ```
