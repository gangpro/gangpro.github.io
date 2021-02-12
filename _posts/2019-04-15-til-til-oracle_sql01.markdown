---
layout: post
title: '[oracle] 오라클 SQL - SELECT'
subtitle: 
categories: til
tags: til oracle sql
comments: true
date: 2019-04-15 01:12:17 +0900
lastmod: 2019-04-15 01:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 SQL SELECT<br />

# 1장. SELECT
> 1장 : SQL SELECT 문을 사용하여 데이터 검색<br>

## SELECT 이해
* 검색, 조회, 질의 ...
* 데이터 분석 도구
* 원하는 집합(결과)을 정의(묘사)하는 언어 


## SELECT 구문 이해
* SELECT 문장 작성 및 해석 권장 순서(절대 실행 순서가 아니다.)
```
    select   : 컬럼, 연산식, 함수, 컬럼 alias, ...   -- 4번째 (필수)
    from     : 재료집합(테이블, 뷰, 서브쿼리)          -- 1번째 (필수)
    where    : 조건, ...                         -- 2번째
    group by : 조건, ...                         -- 3번째
    having   : 조건, ...                         -- 5번째
    order by : 조건, ...                         -- 6번째
```

## 1-5. 기본 SELECT문
```
    SELECT
    *                 : 모든 열을 선택
    FROM table        : 테이블 지정

    SELECT
    A, B              : 특정 열을 선택
    FROM table        : 테이블 지정
    
    [] : Bracket
    {} : Brace
    () : Parentheses
```

## 1-6. SQL Statement 작성 지침
* 참고 : https://orapybubu.blog.me/40023835579
    
## 1-12. 산술 연산 종류
```
    가공 - 연산 - 산술 연산
               - 연결 연산
               - 논리 연산
               - 기타
        - 함수  - Built-in 함수       - 단일행 함수
                                    - 복수행 함수
               - User-defined 함수   - 단일행 함수
                                    - 복수행 함수(가능은 함)
```

## 현재 보유하고 있는 table 모두 리턴
```
    select *
      from tab;
```

## emp 테이블에서 모든 컬럼(*) 리턴
```
    select * 
      from emp;
```

## emp 테이블에서 특정 컬럼(EMPNO, ENAME) 리턴
```
    select EMPNO, ENAME 
      from emp;
```

## 컬럼 복제
* emp 테이블에 sal 컬럼은 한개인데 굳이 컬럼을 원하는 만큼 4개 표시할 수 있다.
```
    select sal, sal, sal, sal
      from emp;
```

## 연산 가능(1)   
* sal합계, sal평균, sal최대값, sal최소값
```
    select sum(sal), avg(sal), max(sal), min(sal)
      from emp;
```

## 연산 가능(2)
* 사원번호 sal, sal 20%인상
```
    select empno, sal, sal*1.2
      from emp;
```

## SQL문 작성
* SQL문은 대소문자를 구분하지 않는다.
  * SELECT * FROM TABLE; <- 사용가능
  * select * from table; <- 사용가능
* SQL문 마지막에는 세미콜론(;)으로 끝내야 한다.
  * select * from table;
  
## 1-14. Null
* unavailable, unassigned, unknown, inapplicable, 비워져있는, 미결정 ...
* ≠ 0 : 0은 null이 아니다.
* ≠ space : 공백은 null이 아니다.
* = null 결과값은 null
* 산술연산 결과값은 null
* 비교연산 결과값은 null
* 논리연산 결과값은 진리표를 참고(2-16, 2-17)
```
    AND T F N     OR T F N     NOT
      T T F N      T T T T       T F 
      F F F F      F T F N       F F 
      N N F N      N T N N       N N
      
       F>N>T        T>N>F
```
       
## 1-14. Null 예제
```
    --null 엉터리 상태 아래와 같이 is null로 써야한다.
    select empno, sal, comm
      from emp
     where comm = null;
    
    --comm이 null값만 출력이 나온다
    select empno, sal, comm
      from emp
     where comm is null;    

    --null에 연산을 해도 null값이 나온다
    select empno, ename, sal, comm, sal*12+comm
      from emp;
    
    --nvl(comm, 0) : comm에 값이 있으면 리턴 없으면 0 이라는 의미
    select empno, ename, sal, comm, sal*12 + nvl(comm, 0)
      from emp;
```

## 1-17. Column alias
```
    --emp table에서
    --empno, ename, sal 컬럼을 가져오고 싶었다
    --ename 뒤에 콤마(,)가 없기 때문에
    --ename alias를 sal로 잘못 잡고 리턴
    select empno, ename sal
      from emp;
```

```
    --alias 처리 방법
    select empno,
           ename,
           sal salary,      --스페이스 하고 난 뒤에 쓸 수 있다. 하지만 헷갈리기 때문에 안 좋은 표현
           sal as salary,   --as를 쓰고 난 뒤에 쓸 수 있다.
           sal "salary",    --특수한 형태를 표현하고 싶을 때 쌍따옴표를 써 그대로 표현 가능 
           sal "$salary"    --특수한 형태를 표현하고 싶을 때 쌍따옴표를 써 그대로 표현 가능  
           from emp;
```

## 1-20, 21. 연결 연산자 & Literal
```
    --emp table에
    --empno 컬럼과
    --ename 컬럼과 
    --'aaa' 컬럼을 만들어서 모든 행에 'aaa' 넣은 후
    --job 컬럼을 리턴 
    select empno, ename, 'aaa', job
      from emp;
```

```
    --emp table에
    --empno 컬럼과
    --ename컬럼+리터럴|| 'aaa' ||+job컬럼을 합쳐서 컬럼명을 sawon으로
    select empno, ename || 'aaa' || job as sawon
      from emp;
```

```
    --emp table에
    --empno 컬럼과
    --ename컬럼+리터럴|| ' IS A ' ||+job컬럼을 합쳐서 컬럼명을 sawon으로
    select empno, ename || ' IS A ' || job as sawon
      from emp;
```

```
    --모든 테이블 삭제  
    select 'drop table' ||tname||' cascade constraints;' as commands
      from tab;
```

```
    --tname이 J로 시작하는 table 삭제 
    select 'drop table' ||tname||' cascade constraints;' as commands
      from tab
     where tname like 'J%';
```

## 1-23. q 연산자
```
    --에러남   문장기호' '데이터                데이터'  '문장기호
    select ename||' 's house is bigger then Tom's '
      from emp
     where sal >= 3000;
```

```
    --에러남   문장기호' '데이터                데이터'   '문장기호
    select ename||' ' 's house is bigger then Tom' 's '
      from emp
     where sal >= 3000;
```

```
    --가능 
    select ename||'''s house is bigger then Tom''s ' as notice
     from emp
     where sal >= 3000;
```

```
    --가능       '아무기호나 가능' 대칭만 맞으면 된다. 
    select ename||q'[  ]' as notice from emp;
    select ename||q'!  !' as notice from emp;
```

```
    --가능
    select ename||q'['s house is bigger then Tom's]' as notice from emp;
```

## 1-24. 중복 제거
```
    --emp table에서
    --job 컬럼만 리턴 
    select job from emp;
```

```
    --emp table에서
    --중복값 제거 후 
    --job 컬럼만 리턴 
    select unique job from emp;
```

```
    --emp table에서
    --중복값 제거 후 
    --job 컬럼만 리턴 
    select distinct job from emp;
```

```
    --emp table에서
    --deptno, job의 중복값 모두 제거 후 리턴
    select distinct deptno, job from emp;
```

## 1-26. 테이블 구조 표시
```
    --desc : describe을 줄인
    --emp table에 대해 Name, Null, Type 등을 알 수 있다.
    desc emp
```

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>