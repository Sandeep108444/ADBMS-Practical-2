-- PART A: Create Normalized Tables

DROP TABLE IF EXISTS Courses;
DROP TABLE IF EXISTS Departments;

CREATE TABLE Departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50) UNIQUE
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES Departments(dept_id)
);
-- PART B: Insert Sample Data

INSERT INTO Departments (dept_id, dept_name) VALUES
(1, 'Civil'),
(2, 'Computer Science'),
(3, 'Electrical'),
(4, 'Electronics'),
(5, 'Mechanical');

INSERT INTO Courses (course_id, course_name, dept_id) VALUES
(101, 'DBMS', 1),
(102, 'Operating Systems', 1),
(103, 'Power Systems', 2),
(104, 'Digital Circuits', 2),
(105, 'Thermodynamics', 3),
(106, 'Fluid Mechanics', 3),
(107, 'Structural Engineering', 4),
(108, 'Surveying', 4),
(109, 'Embedded Systems', 5),
(110, 'VLSI Design', 5);
-- PART C: Departments Offering More Than Two Courses

SELECT D.dept_name AS 'Departments with >2 Courses'
FROM Departments D
WHERE D.dept_id IN (
    SELECT dept_id
    FROM Courses
    GROUP BY dept_id
    HAVING COUNT(course_id) > 2
);
-- PART D: Grant Read-Only Access on Courses Table

-- Make sure the user exists first:
CREATE USER IF NOT EXISTS 'viewer_user'@'localhost' IDENTIFIED BY 'StrongPassword123';

-- Then grant SELECT privilege on the Courses table:
GRANT SELECT ON your_database_name.Courses TO 'viewer_user'@'localhost';

-- (Optional but recommended)
FLUSH PRIVILEGES;


