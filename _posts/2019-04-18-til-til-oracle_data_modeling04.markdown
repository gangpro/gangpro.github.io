---
layout: post
title: '[oracle] 오라클 데이터 모델링 - 제약 조건'
subtitle: 
categories: til
tags: til oracle
comments: true
date: 2019-04-18 04:12:17 +0900
lastmod: 2019-04-18 04:30:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

오라클 데이터 모델링 - 제약 조건<br />

# 제약 조건
> Constraints

## Unique Identifier (식별자)
* UID : A(Attribute 하나), A++(Attribute 두개 이상), A+R(Attribute와 relationship의 조합), R++(relationship 두개 이상)
* UID 찾는 순서
  - 1.Attribute를 이용해서 UID 결정하세요.
  - 2.필요할 경우 Attribute + Relationship으로 UID를 결정하세요.
  - 3.필요할 경우 Artificial Attribute를 이용해서 UID를 결정하세요.
  - cf.1:M 관계에서 1쪽의 UID가 복잡할 경우 Database Design시에 M쪽에 컬럼이 많이 생긴다.
  - [식별자 관계(UID의 일부가 되는 Relationship)]와 [비식별자 관계(UID의 일부가 되지 않는 Relationship)]를 결정

###
    ~ 식별자 관계   -> PK 구조가 복잡해진다. 중복이 많아진다. 

	A : REGIONS 

	a1 (pk)
	a2

	B : COUNTRY

	b1 (pk)
	b2
	a1 (pk), (fk)

	C : DEPT

	c1 (pk)
	c2
	b1 (pk), (fk)
	a1 (pk), (fk)

	D : EMP

	d1 (pk)
	d2
	c1 (pk), (fk)
	b1 (pk), (fk)
	a1 (pk), (fk)
###
    ~ 비식별자 관계 -> 정보가 단절되어 과다한 조인이 필요해진다.

	A : REGIONS 

	a1 (pk)
	a2

	B : COUNTRY

	b1 (pk)
	b2
	a1 (fk)

	C : DEPT

	c1 (pk)
	c2
	b1 (fk)

	D : EMP

	d1 (pk)
	d2
	c1 (fk)

###
    [1] 식별자 관계 : C, D 조인
  
    select e.*
    from dept d, emp e
    where d.deptid = e.deptno
    and d.country_id = e.country_id
    and d.region_id = e.region_id
    and d.name = 'SALES';
###
    [2] 식별자 관계 : A, D 조인

    select e.*
    from region r, emp e
    where r.region_id = e.region_id
    and r.name = 'ASIA';
###
    [3] 비식별자 관계 : C, D 조인
 
    select e.*
    from dept d, emp e
    where and d.deptid = e.deptno
    and d.name = 'SALES';
###
    [4] 비식별자 관계 : A, D 조인

    select e.*
    from region r, country c, dept d, emp e
    where r.region_id = c.region_id
    and c.country_id = d.country_id
    and d.deptid = e.deptno
    and r.name = 'ASIA';

## 4-7

    drop table orders purge;
    drop table customers purge;

    create table customers
    (family_name varchar2(10),
     initials    varchar2(10),
     address     varchar2(30),
     telephone   varchar2(11),
       constraint customers_pk primary key(family_name, address));

    create table orders
    (order_date  date,
     family_name varchar2(10),
     address     varchar2(30),
       constraint orders_pk primary key(order_date, family_name, address),
       constraint orders_fk foreign key(family_name, address) references customers(family_name, address));

    >> 복합PK 컬럼 순서는 주의해서 결정해야 합니다.

       ~ 오라클의 Index는 Rowid를 전문적으로 보관하는 객체(물론 Key 컬럼의 값도 보관함)
       ~ 복합 인덱스 설계시 컬럼 순서를 결정하는 방법 
 
            항상 사용되는 컬럼

                   ↓ 
 
            항상 =로 사용되는 컬럼

                   ↓ 
  
            유일값이 많은 컬럼

                   ↓ 
  
            기타 여러 기준

            <예제>

            where a = 
            and   b = 
            and   c =

            where a = 
            and   c =

            where b like 
            and   c =

            where c =

            도출된 인덱스 : c + a + b

## 4-8

    drop table rooms purge;
    drop table floors purge;
    drop table hotels purge;

    create table hotels
    (name varchar2(10),
       primary key(name));

    create table floors
    (no         number(2),
     hotel_name varchar2(10),
       primary key(no, hotel_name),
       foreign key(hotel_name) references hotels(name));

    create table rooms
    (no         number(2),
     floor_no   number(2),
     hotel_name varchar2(10),
       primary key(no, floor_no, hotel_name),
       foreign key(floor_no, hotel_name) references floors(no, hotel_name));

 



###
    --문제1
    DROP TABLE t_customers PURGE;
    DROP TABLE t_orders    PURGE;
    
    CREATE TABLE t_customers
    (	family_name VARCHAR2(20),
        initials    VARCHAR2(10),
        address     VARCHAR2(30),
        telephone   VARCHAR2(15),
        PRIMARY KEY(family_name, address)
    );
    
    
    CREATE TABLE t_orders
    (	family_name VARCHAR2(20),
        address     VARCHAR2(30),
        order_date  DATE,
        PRIMARY KEY(order_date),
        FOREIGN KEY(family_name, address) REFERENCES t_customers(family_name, address)
    );

<img width="623" alt="Screen Shot 2019-04-17 at 3 07 47 PM" src="https://user-images.githubusercontent.com/46523571/56264798-93534100-6122-11e9-99a2-dd8e8d806742.png">
[사진출처 : 오라클(RDM_vol.1_English)] 


###
    --문제2
    CREATE TABLE a
    (	a1	VARCHAR2(10),
        a2	VARCHAR2(10),
        PRIMARY KEY(a1)
    );
    
    CREATE TABLE b
    (	b1	VARCHAR2(10),
        b2	VARCHAR2(10),
        a1	VARCHAR2(10),
        PRIMARY KEY(b1, b2),
        FOREIGN KEY(a1) REFERENCES a(a1)
    );
    
    CREATE TABLE c
    (	c1	VARCHAR2(10),
        c2	VARCHAR2(10),
        b1	VARCHAR2(10),
        b2	VARCHAR2(10),
        PRIMARY KEY(c1),
        FOREIGN KEY(b1, b2) REFERENCES b(b1, b2)
    );
    
    CREATE TABLE d
    (	d1	VARCHAR2(10),
        d2	VARCHAR2(10),
        c1	VARCHAR2(10),
        PRIMARY KEY(d1),
        FOREIGN KEY(c1) REFERENCES c(c1)
    );

<img width="1255" alt="Screen Shot 2019-04-17 at 3 05 37 PM" src="https://user-images.githubusercontent.com/46523571/56264709-4a02f180-6122-11e9-88b5-a69d8a167f29.png">



###
    --문제3
    CREATE TABLE aa
    (	a1	VARCHAR2(10),
        a2	VARCHAR2(10),
        PRIMARY KEY(a1)
    );
    
    CREATE TABLE bb
    (	b1	VARCHAR2(10),
        b2	VARCHAR2(10),
        a1	VARCHAR2(10),
        PRIMARY KEY(b1, b2, a1),
        FOREIGN KEY(a1) REFERENCES aa(a1)
    );
    
    CREATE TABLE cc
    (	c1	VARCHAR2(10),
        c2	VARCHAR2(10),
        b1	VARCHAR2(10),
        b2	VARCHAR2(10),
        a1	VARCHAR2(10),
        PRIMARY KEY(c1, b1, b2, a1),
        FOREIGN KEY(b1, b2, a1) REFERENCES bb(b1,b2, a1)
    );
    
    CREATE TABLE dd
    (	d1	VARCHAR2(10),
        d2	VARCHAR2(10),
        c1	VARCHAR2(10),
        b1	VARCHAR2(10),
        b2	VARCHAR2(10),
        a1	VARCHAR2(10),
        PRIMARY KEY(d1, c1, b1, b2, a1),
        FOREIGN KEY(c1, b1, b2, a1) REFERENCES cc(c1, b1,b2, a1)
    );
<img width="1193" alt="Screen Shot 2019-04-17 at 3 06 53 PM" src="https://user-images.githubusercontent.com/46523571/56264759-728aeb80-6122-11e9-812f-7a1bd06dec97.png">

<br>
<br>
<br>

## 아크 Arcs
* 아크란는 상호 베타적 relationship이다.
* 일반적으로 relationship은 상호 독립적이나 아크가 생기면서 relationship이 상호 베타적으로 바뀐다.
* ex) 은행계좌 - 법인 고객, 은행계좌 - 개인 고객
<img width="781" alt="Screen Shot 2019-04-17 at 1 29 45 PM" src="https://user-images.githubusercontent.com/46523571/56261147-e1614800-6114-11e9-88f0-d69dd44e743a.png">

## 아크와 서브타입
* 서브타입 -> 아크 변환 무조건 됨
* 아크 -> 서브타입 변환 될 수도 있고 안될 수도 있음.

## Boundaries
* 각각의 entity가 관련성이 하나도 없으며 그냥 참고용으로 만든다.







<br>
<br>
<br>
<br>
<br>

## References
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>