# tables
Students Table:
course table


``` SQL
CREATE TABLE Students (
    StudentID SERIAL PRIMARY KEY,
    StudentName VARCHAR(50)
);
```

``` SQL
CREATE TABLE Courses (
    CourseID SERIAL PRIMARY KEY,
    CourseName VARCHAR(50)
);
```

``` SQL
CREATE TABLE Enrollments (
    EnrollmentID SERIAL PRIMARY KEY,
    EnrollmentDate DATE,
    Grade VARCHAR(2),
    StudentID INT REFERENCES Students(StudentID),
    CourseID INT REFERENCES Courses(CourseID)
);
```

``` SQL
INSERT INTO Students (StudentName) VALUES 
('Alice'),
('Bob'),
('Charlie'),
('David');
```

``` SQL
INSERT INTO Courses VALUES
(101, 'Math'),
(102,'English'),
(103,'Science'),
(104,'History');
```

``` SQL
INSERT INTO Enrollments (EnrollmentDate, Grade, StudentID, CourseID) VALUES
('2022-01-01', 'A', 1, 101),
('2022-01-02', 'B', 2, 102),
('2022-01-03', 'C', 3, 103),
('2022-01-04', 'D', 4, 104);
```

## Write a query to retrieve the names of students who are enrolled in the 'Math' course.
``` SQL
SELECT st.StudentName
FROM Students st
JOIN Enrollments e ON st.StudentID = e.StudentID
JOIN Courses co ON e.CourseID = co.CourseID
WHERE co.CourseName = 'Math';
```

## Write a query to find the names of students who are not enrolled in any course.
``` SQL
SELECT st.StudentName
FROM Students st
LEFT JOIN Enrollments en ON st.StudentID = en.StudentID
WHERE en.StudentID IS NULL;
```

## Write a query to retrieve the names of students who are enrolled in both 'Math' and 'English' courses.
``` SQL
SELECT st.StudentName
FROM Students st
JOIN Enrollments e ON st.StudentID = e.StudentID
JOIN Courses co ON e.CourseID = co.CourseID
WHERE co.CourseName IN ('Math') AND co.CourseName IN ('English')
```

## Write a query to find the names of students who are not enrolled in any course using the NOT IN clause.
``` SQL
SELECT StudentName
FROM Students
WHERE StudentID NOT IN (SELECT StudentID FROM Enrollments);
```

## Write a query to retrieve the names of students who are enrolled in at least one course using the EXISTS clause.
``` SQL
SELECT st.StudentName
FROM Students st
WHERE EXISTS (
    SELECT *
    FROM Enrollments e
    WHERE e.StudentID = st.StudentID
);
```

## Write a query to find the names of students who are not enrolled in any course using the NOT EXISTS clause.
``` SQL
SELECT st.StudentName
FROM Students st
WHERE NOT EXISTS (
    SELECT *
    FROM Enrollments e
    WHERE e.StudentID = st.StudentID
);
```
