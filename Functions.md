# Funkcje

#### 1. Display Student Attendance for a Given Course Module
This function shows the attendance of a student in a specific course module based on the student ID and product ID.


```sql
CREATE FUNCTION Show_Students_Course_Module_Attendance (
    @student_id INT,
    @product_id INT
)
RETURNS TABLE
AS
RETURN
    (
        SELECT * FROM Course_Module_Attendance_List AS AL
        WHERE AL.student_id = @student_id AND AL.product_id = @product_id
    )
```

#### 2. Display Student Attendance for a Given Subject in Studies
This function displays the attendance of a student in a specific subject during their studies, identified by the student ID and product ID.

```sql
CREATE FUNCTION Show_Students_Subject_Attendance (
    @student_id INT,
    @product_id INT
)
RETURNS TABLE
AS
RETURN
    (
        SELECT * FROM Subjects_Attendance_List AS AL
        WHERE AL.student_id = @student_id AND AL.product_id = @product_id
    )
```

#### 3. Display Student Attendance for a Given Internship
This function retrieves the attendance of a student for a specific internship based on the student ID and internship ID.

```sql
CREATE FUNCTION Show_Students_Internship_Attendance (
    @student_id INT,
    @internship_id INT
)
RETURNS TABLE
AS
RETURN
    (
        SELECT * FROM Internship_Attendance_List AS AL
        WHERE AL.student_id = @student_id AND AL.internship_id = @internship_id
    )
```

#### 4. Display Student Meetings
This function returns a list of meeting IDs that a student is part of, based on their product enrollments across various modules, courses, subjects, and internships.

```sql
CREATE FUNCTION GetUserMeetings (@UserID INT)
RETURNS TABLE
AS
RETURN
(
    WITH UserProducts AS (
        SELECT product_id FROM GetStudentsProducts(@UserID)
    )
    SELECT meeting_id
    FROM UserProducts
        INNER JOIN Studies ON Studies.product_id = UserProducts.product_id
        INNER JOIN Subjects ON Subjects.study_id = Studies.product_id
        INNER JOIN Subject_Meetings ON Subject_Meetings.subject_id = Subjects.product_id
    UNION ALL
    SELECT meeting_id
    FROM UserProducts
        INNER JOIN Courses ON Courses.product_id = UserProducts.product_id
        INNER JOIN Course_Modules ON Course_Modules.course_id = Courses.product_id
        INNER JOIN Module_Meetings ON Module_Meetings.course_module_id = Course_Modules.product_id
    UNION ALL
    SELECT meeting_id
    FROM UserProducts
        INNER JOIN Course_Modules ON Course_Modules.product_id = UserProducts.product_id
        INNER JOIN Module_Meetings ON Module_Meetings.course_module_id = Course_Modules.product_id
    UNION ALL
    SELECT meeting_id
    FROM UserProducts
        INNER JOIN Subjects ON Subjects.product_id = UserProducts.product_id
        INNER JOIN Subject_Meetings ON Subject_Meetings.subject_id = Subjects.product_id
    UNION ALL
    SELECT meeting_id
    FROM UserProducts
        INNER JOIN Studies ON Studies.product_id = UserProducts.product_id
        INNER JOIN Internship ON Internship.study_id = Studies.product_id
        INNER JOIN Internship_Meetings ON Internship_Meetings.internship_id = Internship.internship_id
)
go
```

#### 5. Returns a List of Products for a Given Student
This function returns a list of all the products (webinars, courses, studies, course modules, and subjects) a student is enrolled in, identified by the student ID.

```sql
CREATE FUNCTION GetStudentsProducts (@UserID INT)
RETURNS TABLE
AS
RETURN
(
    SELECT * FROM GetStudentsInWebinars
    WHERE users_id = @UserID
    UNION
    SELECT * FROM GetStudentsInCourses
    WHERE users_id = @UserID
    UNION
    SELECT * FROM GetStudentsInStudies
    WHERE users_id = @UserID
    UNION
    SELECT * FROM GetStudentsInCourseModules
    WHERE users_id = @UserID
    UNION
    SELECT * FROM GetStudentsInSubjects
    WHERE users_id = @UserID
)
go
```

#### 6. Check if a Student Completed Their Internship
This function checks if a student has completed their internship by comparing the total number of meetings with the number of meetings the student attended for a specific internship. 
It returns a value indicating whether the student has passed the internship or not.

```sql
CREATE FUNCTION Check_Internship_Completion (
    @student_id INT,
    @internship_id INT
)
RETURNS BIT
AS
BEGIN
    DECLARE @total_meetings INT;
    DECLARE @present_meetings INT;
    DECLARE @passed BIT;

    -- Total meetings for the internship
    SELECT @total_meetings = COUNT(*)
    FROM Internship_Meetings im
    INNER JOIN Internship i ON i.internship_id = im.internship_id
    INNER JOIN Meetings m ON m.meeting_id = im.meeting_id
    INNER JOIN Studies st ON st.product_id = i.study_id
    INNER JOIN GetStudentsInStudies sis ON sis.student_id = @student_id AND sis.product_id = st.product_id
    WHERE i.internship_id = @internship_id;

    -- Total meetings attended by the student for the internship
    SELECT @present_meetings = COUNT(*)
    FROM Internship_Meetings im
    INNER JOIN Internship i ON i.internship_id = im.internship_id
    INNER JOIN Meetings m ON m.meeting_id = im.meeting_id
    INNER JOIN Studies st ON st.product_id = i.study_id
    INNER JOIN GetStudentsInStudies sis ON sis.student_id = @student_id AND sis.product_id = st.product_id
    INNER JOIN Meetings_Presence mp ON sis.student_id = mp.student_id
    WHERE i.internship_id = @internship_id;

    -- Compare the counts
    IF @total_meetings = @present_meetings
        SET @passed = 1;
    ELSE
        SET @passed = 0;

    RETURN @passed;
END;
```
