# assignments
CREATE TABLE staff_master(staff_code number(8) not null,
			staff_name varchar2(50) not null,
			design_code number,
			dept_code number,
			hiredate date,
			staff_dob date,
			staff_address varchar2(240),
			mgr_code number(8),
			staff_sal number(10,2));

insert into staff_master values(&staff_code,'&staff_name',
			&design_code,&dept_code,
			'&hiredate','&staff_dob','&staff_address',
			&mgr_code,&staff_sal);
----------------------------------------------------------------------------------------
1.1.
select staff_name,design_code from staff_master
where (staff_sal between 12000 AND 25000)
AND (hiredate<to_date('1-jan-2003'));

1.2.
select staff_code,staff_name,dept_code from staff_master
where (months_between(sysdate,hiredate)/12)>18
order by (sysdate-hiredate);

1.3.
select staff_code,staff_name,design_code,dept_code,hiredate,staff_dob,staff_address, mgr_code,staff_sal from staff_master where mgr_code IS NULL;
---------------------------------------------------------
CREATE TABLE book_master(book_code number(10) not null,
book_name varchar2(50),book_pub_year number, book_pub_author varchar2(50));

insert into book_master values(&book_code,'&book_name',&book_pub_year,'&book_pub_author');
---------------------------------------------------------
1.4.
select book_code,book_name,book_pub_year,book_pub_author from book_master
where (book_pub_year between 2001 AND 2004) OR (book_name LIKE '%&%');

1.5.
select staff_name from staff_master where staff_name like'%/_%' escape '/';
--------------------------------------------------------------------------------------------------------
2.1
select staff_name,lpad(staff_sal,15,'$') from staff_master;
------------------------------------------------------------------------
CREATE TABLE student_master(student_code number(6) not null,student_name varchar2(50) not null,
dept_code number(2),student_dob date,student_address varchar2(240));

insert into student_master values(&code,'&name','&dept_code','&dob','&address');
------------------------------------------------------------------------------------------------------
2.1.2
SELECT STUDENT_NAME,TO_CHAR(STUDENT_DOB,'MONTH DD YYYY') AS STUDENT_DOB FROM STUDENT_MASTER 
WHERE TO_CHAR(STUDENT_DOB,'DAY') LIKE  ('SATURDAY%') 
OR TO_CHAR(STUDENT_DOB,'DAY') LIKE  ('SUNDAY%') ;
-------------------------------------------------------------------------------------------------
2.1.3
select staff_name, floor(months_between(sysdate,hiredate)) "Months worked" from staff_master
order by (months_between(sysdate,hiredate));

2.1.4
SELECT * FROM STAFFMASTER WHERE TO_CHAR(HIREDATE,'DD') BETWEEN 1 AND 15 
AND TO_CHAR(HIREDATE,'MONTH') LIKE '%DECEMBER%' ;

2.1.5
SELECT STAFF_NAME,STAFF_SAL , 
	CASE 
	WHEN STAFF_SAL >=50000 THEN 'A'  
	WHEN STAFF_SAL  >25000 AND  STAFF_SAL<50000 THEN 'B' 
	WHEN STAFF_SAL  >10000 AND  STAFF_SAL<25000 THEN 'C' 
	ELSE 'D' 
	END CASE
FROM STAFF_MASTER;

2.1.6
SELECT hiredate,TO_CHAR(HIREDATE,'DY')AS DAY FROM STAFF_MASTER 
ORDER BY TO_CHAR(HIREDATE-1,'D') ASC;

2.1.7
select instr('mississippi','i','1','3') from dual;

2.1.8
select to_char(next_day(
last_day(sysdate) - INTERVAL '7' DAY,
 'FRIDAY'
),'Ddthsp" of " Month", "yyyy')from dual;

2.1.9
select student_code,student_name,decode(dept_code,20,'Electricals',
30,'Electronics','others') from student_master;
------------------------------------------------------------------------------------------------------
2.2.1
select max(staff_sal) "Maximum",min(staff_sal) "Minimum", sum(staff_sal) "Total", avg(staff_sal) "Average"
from staff_master;

2.2.2
select dept_code,count(dept_code) "Total number of Managers" from staff_master group by dept_code;
	
2.2.3
select s.dept_code,sum(s.staff_sal) from staff_master s
 join staff_master s1 on s.staff_code =s1.staff_code
 where s.staff_code not in s1.mgr_code
 group by s.dept_code;
---------------------------------------------------------------------------------------------------
