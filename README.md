# SQL Joins and Associations

Here is the terminal code from today:

```
sqlite> .headers on
sqlite> .mode columns
sqlite> .schema
sqlite> CREATE TABLE park (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT
   ...> );
sqlite> DROP TABLE park;
sqlite> CREATE TABLE teachers (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT
   ...> );
sqlite> .schema
CREATE TABLE teachers (
id INTEGER PRIMARY KEY,
name TEXT
);
sqlite> CREATE TABLE students (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT,
   ...> teacher_id INTEGER
   ...> );
sqlite> .schema
CREATE TABLE teachers (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE students (
id INTEGER PRIMARY KEY,
name TEXT,
teacher_id INTEGER
);
sqlite> INSERT INTO teachers (name) VALUES ("Becky");
sqlite> INSERT INTO teachers (name) VALUES ("Jack");
sqlite> INSERT INTO teachers (name) VALUES ("John");
sqlite> SELECT * FROM teachers;
id          name      
----------  ----------
1           Becky     
2           Jack      
3           John      
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Sean", 2);
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Blake the Teacher's Pet", 2);
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Ralph the Class Clown", 2);
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Sydney", 1);
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Bob the Raccoon", 3);
sqlite> SELECT * FROM students;
id          name        teacher_id
----------  ----------  ----------
1           Sean        2         
2           Blake the   2         
3           Ralph the   2         
4           Sydney      1         
5           Bob the Ra  3         
sqlite> SELECT name FROM students;
name      
----------
Sean      
Blake the
Ralph the
Sydney    
Bob the Ra
sqlite> SELECT id, name FROM students;
id          name      
----------  ----------
1           Sean      
2           Blake the
3           Ralph the
4           Sydney    
5           Bob the Ra
sqlite> SELECT student.id, student.name FROM students;
Error: no such column: student.id
sqlite> SELECT student.id, student.name FROM students
   ...> SELECT student.id, student.name FROM students;
Error: near "SELECT": syntax error
sqlite> SELECT * FROM students
   ...> JOIN teachers ON students.teacher_id = teachers.id
   ...> ;
id          name        teacher_id  id          name      
----------  ----------  ----------  ----------  ----------
1           Sean        2           2           Jack      
2           Blake the   2           2           Jack      
3           Ralph the   2           2           Jack      
4           Sydney      1           1           Becky     
5           Bob the Ra  3           3           John      
sqlite> SELECT name from STUDENTS;
name      
----------
Sean      
Blake the
Ralph the
Sydney    
Bob the Ra
sqlite> SELECT name from STUDENTS
   ...> JOIN teachers ON student.teacher_id = teacher.id;
Error: ambiguous column name: name
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN student.teacher_id = teacher.id;
Error: near "=": syntax error
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN teachers ON student.teacher_id = teacher.id;
Error: no such column: student.teacher_id
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN teachers ON students.teacher_id = teachers.id;
name        name      
----------  ----------
Sean        Jack      
Blake the   Jack      
Ralph the   Jack      
Sydney      Becky     
Bob the Ra  John      
sqlite> insert into students (name) values ("Rocky");
sqlite> INSERT INTO students (name, teacher_id)
   ...> VALUES ("Jason", 1);
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN teachers ON students.teacher_id = teachers.id;
name        name      
----------  ----------
Sean        Jack      
Blake the   Jack      
Ralph the   Jack      
Sydney      Becky     
Bob the Ra  John      
Jason       Becky     
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN teachers ON students.teacher_id = teachers.id
   ...> ORDER BY students.teacher_id;
name        name      
----------  ----------
Sydney      Becky     
Jason       Becky     
Sean        Jack      
Blake the   Jack      
Ralph the   Jack      
Bob the Ra  John      
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> JOIN teachers ON students.teacher_id = teachers.id
   ...> ORDER BY teachers.id;
name        name      
----------  ----------
Sydney      Becky     
Jason       Becky     
Sean        Jack      
Blake the   Jack      
Ralph the   Jack      
Bob the Ra  John      
sqlite> SELECT * FROM students;
id          name        teacher_id
----------  ----------  ----------
1           Sean        2         
2           Blake the   2         
3           Ralph the   2         
4           Sydney      1         
5           Bob the Ra  3         
6           Rocky                 
7           Jason       1         
sqlite> SELECT students.name, teachers.name from STUDENTS
   ...> LEFT JOIN teachers ON students.teacher_id = teachers.id
   ...> ORDER BY teachers.id;
name        name      
----------  ----------
Rocky                 
Sydney      Becky     
Jason       Becky     
Sean        Jack      
Blake the   Jack      
Ralph the   Jack      
Bob the Ra  John      
sqlite> SELECT * FROM teachers;
id          name      
----------  ----------
1           Becky     
2           Jack      
3           John      
sqlite> SELECT * FROM teachers
   ...> JOIN students ON teachers.id = students.teacher_id;
id          name        id          name        teacher_id
----------  ----------  ----------  ----------  ----------
2           Jack        1           Sean        2         
2           Jack        2           Blake the   2         
2           Jack        3           Ralph the   2         
1           Becky       4           Sydney      1         
3           John        5           Bob the Ra  3         
1           Becky       7           Jason       1         
sqlite> SELECT * FROM teachers
   ...> JOIN students ON teachers.id = students.teacher_id
   ...> WHERE teachers.id = 2 AND teachers.name = "Jack";
id          name        id          name        teacher_id
----------  ----------  ----------  ----------  ----------
2           Jack        1           Sean        2         
2           Jack        2           Blake the   2         
2           Jack        3           Ralph the   2         
sqlite> SELECT teacher_id FROM students
   ...> JOIN teachers ON students.teacher_id = teachers.id;
teacher_id
----------
2         
2         
2         
1         
3         
1         
sqlite> CREATE TABLE people (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT
   ...> );
sqlite> CREATE TABLE banks (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT
   ...> );
sqlite> CREATE TABLE accounts (
   ...> id INTEGER PRIMARY KEY,
   ...> amount REAL
   ...> );
sqlite> DROP TABLE accounts;
sqlite> CREATE TABLE accounts (
   ...> id INTEGER PRIMARY KEY,
   ...> amount REAL
   ...> ,
   ...> bank_id INTEGER,
   ...> person_id INTEGER
   ...> );
sqlite> .schema
CREATE TABLE teachers (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE students (
id INTEGER PRIMARY KEY,
name TEXT,
teacher_id INTEGER
);
CREATE TABLE people (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE banks (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE accounts (
id INTEGER PRIMARY KEY,
amount REAL
,
bank_id INTEGER,
person_id INTEGER
);
sqlite> CREATE TABLE whatevers (
   ...> id INTEGER PRIMARY KEY,
   ...> person_id INTEGER FOREIGN KEY
   ...> );
Error: near "FOREIGN": syntax error
sqlite> .schema
CREATE TABLE teachers (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE students (
id INTEGER PRIMARY KEY,
name TEXT,
teacher_id INTEGER
);
CREATE TABLE people (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE banks (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE accounts (
id INTEGER PRIMARY KEY,
amount REAL
,
bank_id INTEGER,
person_id INTEGER
);
sqlite> INSERT INTO people (name) VALUES ("Blake");
sqlite> INSERT INTO people (name) VALUES ("Mary");
sqlite> INSERT INTO people (name) VALUES ("Todd");
sqlite> INSERT INTO banks (name) VALUES ("TD Bank");
sqlite> INSERT INTO banks (name) VALUES ("Chase");
sqlite> INSERT INTO accounts (amount, bank_id, person_id)
   ...> VALUES (100.00, 1, 1);
sqlite> INSERT INTO accounts (amount, bank_id, person_id)
   ...> VALUES (0.01, 2, 1);
sqlite> SELECT * FROM accounts
   ...> JOIN banks ON accounts.bank_id = banks.id;
id          amount      bank_id     person_id   id          name      
----------  ----------  ----------  ----------  ----------  ----------
1           100.0       1           1           1           TD Bank   
2           0.01        2           1           2           Chase     
sqlite> SELECT * FROM accounts
   ...> JOIN people ON accounts.people_id = people.id;
Error: no such column: accounts.people_id
sqlite> SELECT * FROM accounts
   ...> JOIN people ON accounts.person_id = people.id;
id          amount      bank_id     person_id   id          name      
----------  ----------  ----------  ----------  ----------  ----------
1           100.0       1           1           1           Blake     
2           0.01        2           1           1           Blake     
sqlite> SELECT * FROM accounts
   ...> JOIN people ON accounts.person_id = people.id
   ...> JOIN banks ON accounts.bank_id = banks.id;
id          amount      bank_id     person_id   id          name        id          name      
----------  ----------  ----------  ----------  ----------  ----------  ----------  ----------
1           100.0       1           1           1           Blake       1           TD Bank   
2           0.01        2           1           1           Blake       2           Chase     
sqlite> SELECT * FROM accounts
   ...> INNER JOIN people ON accounts.person_id = people.id;
id          amount      bank_id     person_id   id          name      
----------  ----------  ----------  ----------  ----------  ----------
1           100.0       1           1           1           Blake     
2           0.01        2           1           1           Blake     
sqlite> SELECT accounts.id, accounts.amount, people.name, banks.name FROM accounts
   ...> JOIN people ON accounts.person_id = people.id
   ...> JOIN banks ON accounts.bank_id = banks.id;
id          amount      name        name      
----------  ----------  ----------  ----------
1           100.0       Blake       TD Bank   
2           0.01        Blake       Chase     
sqlite> SELECT * FROM people
   ...> JOIN accounts ON people.id = accounts.person_id;
id          name        id          amount      bank_id     person_id
----------  ----------  ----------  ----------  ----------  ----------
1           Blake       1           100.0       1           1         
1           Blake       2           0.01        2           1         
sqlite> SELECT * FROM people
   ...> JOIN banks ON people.id = banks.id;
id          name        id          name      
----------  ----------  ----------  ----------
1           Blake       1           TD Bank   
2           Mary        2           Chase     
sqlite> SELECT * FROM people
   ...> JOIN accounts ON people.id = accounts.person_id;
id          name        id          amount      bank_id     person_id
----------  ----------  ----------  ----------  ----------  ----------
1           Blake       1           100.0       1           1         
1           Blake       2           0.01        2           1         
sqlite> SELECT people.name, banks.name FROM people
   ...> JOIN accounts ON people.id = accounts.person_id
   ...> JOIN banks ON accounts.bank_id = banks.id;
name        name      
----------  ----------
Blake       TD Bank   
Blake       Chase     
sqlite> SELECT people.name, banks.name FROM people
   ...> JOIN banks ON accounts.bank_id = banks.id
   ...> JOIN accounts ON people.id = accounts.person_id;
name        name      
----------  ----------
Blake       TD Bank   
Blake       Chase     
sqlite> .quit
```
