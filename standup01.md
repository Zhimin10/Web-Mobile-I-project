# Exercise 3:  Stand Up An RDBMS
By Zhimin Lin       
10/18/2019

#### Method for loding database
1. Line 7 - Changed "USE standup" to "\c standup"
Reason: In postgreSQL, need to use \c to access to a database.
Resource: https://stackoverflow.com/questions/10335561/use-database-name-command-in-postgresql


2. Line 35 - Changed "AUTO_INCREMENT" to "SERIAL"
Reason: AUTO_INCREMENT is a keyword in mySQL, should use SERIAL in postgreSQL
Resource: https://stackoverflow.com/questions/20781111/postgresql-9-1-primary-key-autoincrement

3. Line 58 - Changed “” to ‘’
Reason: In postgreSQL, double quote is use for name of table or field, single quote is use for string. 
Resource: https://stackoverflow.com/questions/41396195/what-is-the-difference-between-single-quotes-and-double-quotes-in-postgresql

#### Questions
Question 1: 
```sql
SELECT result.lan_name AS "First Language" 
FROM
(
	SELECT COUNT(*) AS freq, L.LANGUAGE as lan_name 
	FROM LANGUAGE AS L JOIN USERLANG AS UL 
	ON L.LANGUAGE_ID = UL.LANGUAGE_ID 
	GROUP BY L.LANGUAGE_ID ORDER BY freq DESC LIMIT 1
) AS result
; 
```

First Language |
----------------|
 JAVA|
(1 row)|

Question 2: 
```sql
SELECT L.LANGUAGE AS "Language", SUL.s_name AS "Skill Level", SUL.freq AS "Count"
FROM LANGUAGE AS L JOIN
(
	SELECT S.VALUE AS s_name, UL.LANGUAGE_ID AS l_id, S.SKILL_ID AS s_id, COUNT(*) AS freq
	FROM SKILL AS S JOIN USERLANG AS UL on S.SKILL_ID = UL.SKILL_LEVEL 
	GROUP BY UL.LANGUAGE_ID, S.VALUE, S.SKILL_ID
) AS SUL
ON SUL.l_id = L.LANGUAGE_ID
ORDER BY L.LANGUAGE, SUL.s_id
;
```

Language  | Skill Level | Count 
------------|-------------|-------
 C          | HIGH        |     1
 C          | MEDIUM      |     2
 C          | LOW         |     7
 C#         | HIGH        |     1
 C#         | MEDIUM      |     2
 C#         | LOW         |     2
 C++        | MEDIUM      |    22
 C++        | LOW         |     4
 COBOL      | MEDIUM      |     1
 JAVA       | HIGH        |    34
 JAVA       | MEDIUM      |     1
 JAVASCRIPT | HIGH        |     1
 JAVASCRIPT | MEDIUM      |     4
 OTHER      | HIGH        |     1
 OTHER      | MEDIUM      |     3
 OTHER      | LOW         |     7
 OTHER      | ULTRA LOW   |     2
 PHP        | HIGH        |     1
 PHP        | MEDIUM      |     3
 PHP        | LOW         |     3
 PYTHON     | HIGH        |     1
 PYTHON     | MEDIUM      |     1
 PYTHON     | LOW         |     2
(23 rows)|

Question 3: 
```sql
SELECT S1.STUDENT_ID AS "student_id", S1.lan AS "high", S2.lan AS "medium"
FROM
(
	SELECT STUDENT_ID, LANGUAGE.LANGUAGE as lan
	FROM USERLANG JOIN LANGUAGE 
	ON LANGUAGE.LANGUAGE_ID = USERLANG.LANGUAGE_ID
	where SKILL_LEVEL = 1
) as S1
LEFT JOIN
(
	SELECT STUDENT_ID, LANGUAGE.LANGUAGE as lan
	FROM USERLANG JOIN LANGUAGE 
	ON LANGUAGE.LANGUAGE_ID = USERLANG.LANGUAGE_ID
	where SKILL_LEVEL = 2
)as S2
ON S1.STUDENT_ID = S2.STUDENT_ID;
```

 student_id |    high    |   medium   
------------|------------|------------
          1 | JAVA       | C++
          2 | JAVA       | C++
          3 | JAVA       | C++
          4 | JAVA       | C++
          5 | JAVA       | C++
          6 | JAVA       | C++
          7 | JAVA       | C++
          8 | JAVA       | C++
          9 | JAVA       | C++
         10 | JAVA       | C++
         11 | JAVA       | PYTHON
         12 | JAVA       | C++
         13 | JAVA       | C++
         14 | JAVA       | C++
         15 | JAVA       | C#
         16 | OTHER      | 
         17 | JAVA       | JAVASCRIPT
         18 | PHP        | OTHER
         19 | JAVASCRIPT | PHP
         20 | C          | C++
         21 | PYTHON     | JAVA
         22 | JAVA       | JAVASCRIPT
         23 | JAVA       | OTHER
         24 | JAVA       | PHP
         25 | JAVA       | C
         26 | JAVA       | PHP
         27 | JAVA       | C#
         28 | JAVA       | COBOL
         29 | JAVA       | C++
         30 | JAVA       | C++
         31 | JAVA       | C++
         32 | JAVA       | C++
         33 | JAVA       | C++
         34 | JAVA       | JAVASCRIPT
         35 | C#         | C++
         36 | JAVA       | JAVASCRIPT
         37 | JAVA       | C++
         38 | JAVA       | C
         39 | JAVA       | C++
         40 | JAVA       | OTHER
(40 rows)|







