# Data Normalization and Entity-Relationship Diagramming

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |


## Third Norm Violations

- assuming assignment_id is the primary key, then classroom does not describe assignment
- assuming student_id is the primary key, then grade, classroom, and professor do not describe student

## Fourth Norm Violation
- Due dates, assignment_topic, classroom, and professor are multi-valued facts 
- EX: A student can have multiple professors, classroom, assignments, and due dates







## ER Diagram

![Pushing work in Visual Studio Code](./images/diagram.drawio.svg)


## Student 


### Student Table

| student_id (pk) | student_name  |     Year |
| :------------ | :--------- | :------------ |
|         1     |     John Danes  |    2   |
|         2  |     Curtis Yang    |    4   |

Now we have a basic student table with the pimary key of student id. This doesn't violate any of the norms.

### Student assignment table
| id (pk) | student_id (fk) | assignment_id (fk)|
| :------------ | :--------- | :--------- |
| 1 | 1 | 1 |
| 2 | 1| 3|
| 3 | 2 | 2|

We seperate assignments into another table, where student and assignments are foreign keys to relate students to assignments. Now join statements can be used to match student to assignment details. This avoids redunancy and helps to avoid the 4th and 3rd norm.

### Student section table
| id (pk) | student_id (fk) |  section_id (fk) | 
| :------------ | :--------- | :--------- |
| 1 | 1 | 3 |
| 2 | 2 | 2 |

Similar story to assignments, join statements can be used to match students to section details.

## Grade

| id (pk)| assignment_id (fk) | student_id (fk) | grade |
| :------------ | :---------------- | :----------- |  :-------- | 
| 1             |  2                | 1            |   90       |

Grades are seperated because they describe assignments and not the student directly. This is also done because an assignment doesn't recieve a grade as soon as it's submitted, so a seperate table is needed to avoid a null value.

## Courses

| course_id (pk) | course_name |
| :--------- | :--------- |
|   1        |  Math      |
|   2        |  History   |

A simple table describing courses with their course name.

## Sections

| section_id (pk) | course_id (fk) | classroom | 
| :------------ | :--------- |  :--------- |
| 1             |    1       | West Hall   |
| 2             |   1        | East Hall   |
| 3             |  2         | North Hall  |

A table of sections along with their respective courses, because courses have multiple sections. But each section has one classroom.

## Assignments 

| assignment_id (pk) | section_id (fk) | due_date | Assignment Topic | Relevant Reading |
| :------------ | :------- | :-------- |       :--------  | :--------  |
| 1             |  1       |  9/12     |  Calculus         | Math Textbook |
| 2             |  2       | 9/13      |    Calculus      | Integration for dummies|
| 3             |  3       | 9/15      |   Classic books          | shakespeare|

An assignment along with its corresponding section, due date and assignment topic, and a relevant reading. Join statements can be used against this table to find out which student has which assignment or which course the assignment belongs to. This way, all fields relate directly to the pk (an assignment)


## Professor

| professor_id (pk) | professor_email   | professor_name
| :-------- | :------------------ |  :--------| 
|  1        | 1@email.com         | John Snow |
|  2        | 2@email.com         | Joe Smoe |

Each field directly related to primary key. 

| id (pk) | professor_id (fk) | section_id (fk)| 
| :-------- | :-------- | :-------------|
|   1       |     1     |      2        |
|   2      |     1     |      1   |
|   3      |     2     |      3   |

Avoids redundacy since a professor may handle multiple sections.

