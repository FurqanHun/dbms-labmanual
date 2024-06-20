<h1 style="text-align: center;">Lab Tasks DBMS - MySQL</h1>

# Table Of Content
- [Lab 01 - Implementation of DDL commands](#lab-01)
- [Lab 02 - Implementation of DML commands](#lab-02)
- [Lab 03 - Implementation of Different Types of Operators](#lab-03)
- [Lab 04 - Implementation of Different Types of Joins](#lab-04)
- [Lab 05 - Implementation of Subqueries](#lab-05)
- [Lab 06 - Implementation of Count, Min, Sum](#lab-06)
- [Lab 07 - Implementation of In, Between, Aliases](#lab-07)
- [Lab 08 - Implementation of Union](#lab-08)
- [Lab 09 - Implementation of MIN, MAX](#lab-09)
- [Lab 10 - Implementation of Like, Wildcards](#lab-10)

---

# Lab 01
Implementation of DDL commands of SQL
- Create a table
- Alter table
- Drop Table

## 1. Create a table
```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT NOT NULL
);
```

## 2. Alter table
```sql
ALTER TABLE students
ADD COLUMN email VARCHAR(100);
```

```sql
ALTER TABLE students
CHANGE COLUMN age student_age INT;
```

## 3. Drop Table
``` sql
DROP TABLE students;
```
---
# Lab 02
Implementation of DML commands of SQL
- Insert
- Update
- Delete

## 1. Insert Data

```sql
INSERT INTO students (name, student_age, email)
VALUES ('Furqan', 21, 'qan@thisdoesnotexist.com');
```

## 2. Update Data

```sql
UPDATE students
SET student_age = 20, email = 'qan@stilldoesntexist.com'
WHERE id = 1;
```

## 3. Delete Data

```sql
DELETE FROM students
WHERE id = 1;
```

# Lab 03
Implementation of different types of Operators in SQL
- Arithmetic Operators
- Logical Operators
- Comparison Operator

## 1. Arithmetic Operators

- Adding a value to the `student_age` column:

```sql
SELECT name, student_age, student_age + 1 AS age_next_year
FROM students;
```

- Calculating the product of two columns:

```sql
SELECT id, name, student_age, student_age * 2 AS double_age
FROM students;
```

## 2. Logical Operators

- Selecting students who are older than 18 and younger than 25:

```sql
SELECT name, student_age
FROM students
WHERE student_age > 18 AND student_age < 25;
```
- Selecting students whose name starts with 'J' or are older than 21:

```sql
SELECT name, student_age
FROM students
WHERE name LIKE 'J%' OR student_age > 21;
```
## 3. Comparison Operators

- Selecting students who are exactly 20 years old:

```sql
SELECT name, student_age
FROM students
WHERE student_age = 20;
```

- Selecting students who are not 20 years old:

```sql
SELECT name, student_age
FROM students
WHERE student_age != 20;
```

- Selecting students who are older than 18:

```sql
SELECT name, student_age
FROM students
WHERE student_age > 18;
```

---

# Lab 04
Implementation of different types of joins
- Inner join
- Left join
- Right join

First lets create a table called `courses`
```sql
CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(50) NOT NULL,
    student_id INT
);

INSERT INTO courses (course_name, student_id)
VALUES
    ('Pirate Training', 2),  -- Luffy
    ('Swordsmanship', 3),    -- Zoro
    ('Software Engineering', 1); -- Furqan
```

## 1. Inner Join

```sql
SELECT students.name, students.student_age, courses.course_name
FROM students
INNER JOIN courses ON students.id = courses.student_id;
```

## 2. Left Join

```sql
SELECT students.name, students.student_age, courses.course_name
FROM students
LEFT JOIN courses ON students.id = courses.student_id;
```

## 3. Right Join

```sql
SELECT students.name, students.student_age, courses.course_name
FROM students
RIGHT JOIN courses ON students.id = courses.student_id;
```
---

# Lab 05
Study and implementation of Sub queries
- Select
- Where
- From

## 1. Select

```sql
SELECT name, student_age,
       (SELECT COUNT(*) 
        FROM courses 
        WHERE courses.student_id = students.id) AS course_count
FROM students;
```

## 2. Where

```sql
SELECT name, student_age
FROM students
WHERE id IN (SELECT student_id 
             FROM courses 
             WHERE course_name = 'Pirate Training');
```

## 3. From

```sql
SELECT course_name, AVG(student_age) AS avg_age
FROM (SELECT students.name, students.student_age, courses.course_name
      FROM students
      INNER JOIN courses ON students.id = courses.student_id) AS course_students
GROUP BY course_name;
```
---

# Lab 06

Implementation of
- Count
- Min
- Sum

Before we continue lets add more data in the databse as i realized we didnt have enough data to work with in the previous labs.

```sql
-- Insert more data into the students table
INSERT INTO students (name, student_age, email)
VALUES 
    ('Nami', 18, 'nami@navigator.com'),
    ('Usopp', 19, 'usopp@sniper.com'),
    ('Robin', 30, 'robin@archaeologist.com'),
    ('Chopper', 17, 'chopper@doctor.com'),
    ('Brook', 90, 'brook@musician.com'),
    ('Franky', 36, 'franky@shipwright.com');

-- Insert more data into the courses table
INSERT INTO courses (course_name, student_id)
VALUES
    ('Navigation', 4),  -- Nami
    ('Sniping', 5),     -- Usopp
    ('History', 6),     -- Robin
    ('Medical Training', 7),  -- Chopper
    ('Music', 8),             -- Brook
    ('Engineering', 9),       -- Franky
    ('Swordsmanship', 1),     -- Furqan, adding Furqan to Swordsmanship as well
    ('Cooking', 2),           -- Adding Luffy to Cooking (ifykyk)
    ('Navigation', 3);        -- Adding Zoro to Navigation (lol)
```
## 1. Count

```sql
SELECT COUNT(*) AS total_students
FROM students;
```

## 2. Min

```sql
SELECT MIN(student_age) AS youngest_student
FROM students;
```

## 3. Sum

```sql
SELECT SUM(student_age) AS total_age
FROM students;
```
---

# Lab 07

Implementation of
- In
- Between
- Aliases

## 1. In

```sql
SELECT name, student_age
FROM students
WHERE student_age IN (19, 20, 21);
```
## 2. Between

```sql
SELECT name, student_age
FROM students
WHERE student_age BETWEEN 18 AND 25;
```

## 3. Aliases

```sql
SELECT name AS student_name, student_age AS age
FROM students;
```

---

# Lab 08
Implementation of
- Union

## 1. Union

```sql
SELECT name
FROM students
JOIN courses ON students.id = courses.student_id
WHERE course_name = 'Swordsmanship'

UNION

SELECT name
FROM students
JOIN courses ON students.id = courses.student_id
WHERE course_name = 'Navigation';
```

# Union All

```sql
SELECT name
FROM students
JOIN courses ON students.id = courses.student_id
WHERE course_name = 'Swordsmanship'

UNION ALL

SELECT name
FROM students
JOIN courses ON students.id = courses.student_id
WHERE course_name = 'Navigation';
```

---

# Lab 09
Implementation of
- MIN
- MAX

## 1. MIN

```sql
SELECT MIN(student_age) AS min_age
FROM students;
```

## 2. MAX

```sql
SELECT MAX(student_age) AS max_age
FROM students;
```

---

# Lab 10
Implementation of
- Like
- Wildcards

## 1. Like

```sql
SELECT name
FROM students
WHERE name LIKE 'L%';
```

## 2. Wildcards

```sql
SELECT name
FROM students
WHERE name LIKE '_o%';
```

---

<h1 style="text-align: center;">THE END</h1>
