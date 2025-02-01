# Views

#### 1. Debtors List

```sql
CREATE VIEW Debtors_List AS
    SELECT
        u.user_id,
        u.firstname,
        u.lastname,
        p.product_title,
        od.price - SUM(pay.payment_amount) AS debt
    FROM
        dbo.Orders o
        JOIN dbo.Order_Details od ON o.order_id = od.order_id
        JOIN dbo.Users AS u ON o.user_id = u.user_id
        JOIN dbo.Products AS p ON od.product_id = p.product_id
        JOIN dbo.Payments AS pay ON pay.order_id = od.order_id
    GROUP BY u.user_id, u.firstname, u.lastname, p.product_title, od.price
    HAVING (od.price - SUM(pay.payment_amount) < 0);
```

#### 2. Number of People Registered for Future Events (Internships, Courses, Studies, Webinars)

```sql
CREATE VIEW Future_Event_Participants AS
	SELECT
		ap.product_id,
		ap.product_title,
		ap.current_members,
		CASE
			WHEN ap.product_type LIKE 'Webinar' OR
			(ap.product_type LIKE 'Course_Module' AND mm.meeting_type LIKE 'Online') OR
			(ap.product_type LIKE 'Subject' AND sm.meeting_type LIKE 'Online')
			THEN 'Online'
			ELSE 'On-site'
		END AS event_type,
		p.release_date
	FROM Available_Products ap
	JOIN Products p ON ap.product_id = p.product_id
	LEFT JOIN Course_Modules cm ON p.product_id = cm.product_id
	LEFT JOIN Module_Meetings mm ON cm.course_id = mm.meeting_id
	LEFT JOIN Subjects s ON p.product_id = s.product_id
	LEFT JOIN Subject_Meetings sm ON s.product_id = sm.meeting_id
	WHERE p.release_date > GETDATE() AND (ap.product_type LIKE 'Webinar'
	OR ap.product_type LIKE 'Course_Module' OR ap.product_type LIKE 'Subject');
```

#### 3. Attendance Summary for All Internships

```sql
CREATE VIEW Internship_Attendance AS
	SELECT
        p.product_title,
        i.internship_id,
        COUNT(mp.student_id) AS attendance_count
	FROM
        Internship i
    JOIN
		Meetings m ON m.meeting_id = i.internship_id
    JOIN
		Meetings_Presence mp ON m.meeting_id = mp.meeting_id
    JOIN
	    Products p ON p.product_id = i.study_id
	GROUP BY p.product_title, i.internship_id;
```

#### 4. Attendance Summary for All Courses Modules

```sql
CREATE VIEW Course_Modules_Attendance AS
    SELECT
        p.product_title,
        mm.meeting_id,
        COUNT(mp.student_id) AS attendance_count
    FROM
        Course_Modules cm
    JOIN
        Module_Meetings mm ON cm.product_id = mm.course_module_id
    JOIN
        Meetings_Presence mp ON mm.meeting_id = mp.meeting_id
    JOIN
        Products p ON p.product_id = cm.product_id
    GROUP BY
        p.product_title, mm.meeting_id;
```

#### 5. Attendance Summary for All Subjects

```sql
CREATE VIEW Subjects_Attendance AS
	SELECT
		p.product_title,
		sm.meeting_id,
		COUNT(mp.student_id) AS attendance_count
	FROM
		Subjects s
	JOIN
		Subject_Meetings sm ON s.product_id = sm.subject_id
	JOIN
		Meetings_Presence mp ON sm.meeting_id = mp.meeting_id
	JOIN
		Products p ON p.product_id = s.product_id
	GROUP BY
		p.product_title, sm.meeting_id;
```

#### 6. List of Available Products

```sql
CREATE VIEW Available_Products AS
    WITH ProductPlaceLimits AS (
        SELECT product_id, place_limits AS available_slots, "Course" as product_type
        FROM dbo.Courses AS c
        UNION ALL
        SELECT product_id, place_limits AS available_slots, "Studies" as product_type
        FROM dbo.Studies AS s
        UNION ALL
        SELECT product_id, place_limits AS available_slots, "Course_Module" as product_type
        FROM dbo.Course_Modules AS cm
        UNION ALL
        SELECT product_id, place_limits AS available_slots, "Subject" as product_type
        FROM dbo.Subjects AS sm
        UNION ALL
        SELECT product_id, 2147483647 AS available_slots, "Webinar" as product_type
        FROM dbo.Webinars AS w)
    SELECT p.product_id, p.product_title, ppl.product_type, COUNT(od.order_id) AS current_members, ppl.available_slots - COUNT(od.order_id) AS available_slots
    FROM dbo.Products AS p
    INNER JOIN ProductPlaceLimits AS ppl ON ppl.product_id = p.product_id
    LEFT OUTER JOIN dbo.Order_Details AS od ON p.product_id = od.product_id
    LEFT OUTER JOIN dbo.Orders AS o ON od.order_id = o.order_id
    GROUP BY p.product_id, p.product_title, ppl.available_slots
    HAVING (ppl.available_slots - COUNT(od.order_id) > 0)
```

#### 7. Summary of Total Earnings for Each Product in a Given Category

```sql
CREATE VIEW Webinars_Financial_Raport AS
	SELECT
		p.product_id,
		p.product_title,
		COALESCE(SUM(pay.payment_amount), 0) AS total_revenue
	FROM
		Webinars w
	LEFT JOIN
		Products p ON w.product_id = p.product_id
	LEFT JOIN
		Payments pay ON p.product_id = pay.product_id
	GROUP BY
		p.product_id, p.product_title;
```

```sql
CREATE VIEW Subjects_Financial_Raport AS
	SELECT
		p.product_id,
		p.product_title,
		COALESCE(SUM(pay.payment_amount), 0) AS total_revenue
	FROM
		Subjects s
	LEFT JOIN
		Products p ON s.product_id = p.product_id
	LEFT JOIN
		Payments pay ON p.product_id = pay.product_id
	GROUP BY
		p.product_id, p.product_title;
```

```sql
CREATE VIEW Studies_Financial_Raport AS
	SELECT
		p.product_id,
		p.product_title,
		COALESCE(SUM(pay.payment_amount), 0) AS total_revenue
	FROM
		Studies s
	LEFT JOIN
		Products p ON s.product_id = p.product_id
	LEFT JOIN
		Payments pay ON p.product_id = pay.product_id
	GROUP BY
		p.product_id, p.product_title;
```

```sql
CREATE VIEW Courses_Financial_Raport AS
	SELECT
		p.product_id,
		p.product_title,
		COALESCE(SUM(pay.payment_amount), 0) AS total_revenue
	FROM
		Courses c
	LEFT JOIN
		Products p ON c.product_id = p.product_id
	LEFT JOIN
		Payments pay ON p.product_id = pay.product_id
	GROUP BY
		p.product_id, p.product_title;
```

```sql
CREATE VIEW Course_Modules_Financial_Raport AS
	SELECT
		p.product_id,
		p.product_title,
		COALESCE(SUM(pay.payment_amount), 0) AS total_revenue
	FROM
		Course_Modules cm
	LEFT JOIN
		Products p ON cm.product_id = p.product_id
	LEFT JOIN
		Payments pay ON p.product_id = pay.product_id
	GROUP BY
		p.product_id, p.product_title;
```

#### 8. List of Payments for All Users

```sql
CREATE VIEW User_Product_Payments AS
	SELECT o.user_id, o.order_id, pro.product_title, pay.payment_amount, pay.payment_date,
		   COALESCE(SUM(pay.payment_amount), 0) AS TotalPaid, (od.price - COALESCE(SUM(pay.payment_amount), 0)) AS AmountDue
	FROM dbo.Payments pay
	INNER JOIN dbo.Order_Details od ON od.order_id = pay.order_id
	INNER JOIN dbo.Orders o ON o.order_id = od.order_id
	INNER JOIN dbo.Products pro ON pro.product_id = od.product_id
	GROUP BY o.user_id, o.order_id, pro.product_title, pay.payment_amount, pay.payment_date, od.price
```

#### 9. List of Webinars and Students Attending Them

```sql
CREATE VIEW GetStudentsInWebinars
AS
SELECT DISTINCT s.users_id, u.firstname, u.lastname, od.product_id, s.student_id
FROM Students s
INNER JOIN Users u ON s.users_id = u.user_id
INNER JOIN Orders o ON o.user_id = u.user_id
INNER JOIN Order_Details od ON o.order_id = od.order_id
INNER JOIN Products p ON p.product_id = od.product_id
INNER JOIN Webinars w ON w.product_id = p.product_id
WHERE ((od.order_paid = 1 AND od.due_date >= od.payment_date) OR od.deferred = 1)
AND (DATEADD(DAY, -30, GETDATE()) <= p.start_date)
go
```

#### 10. List of Courses and Students Attending Them

```sql
CREATE view dbo.GetStudentsInCourses as
    SELECT DISTINCT s.users_id,
                    u.firstname,
                    u.lastname,
                    od.product_id,
                    s.student_id
    FROM Students s
             INNER JOIN Users u
                        ON s.users_id = u.user_id
             INNER JOIN Orders o
                        ON o.user_id = u.user_id
             INNER JOIN Order_Details od
                        ON o.order_id = od.order_id
             INNER JOIN Products p
                        ON p.product_id = od.product_id
             INNER JOIN Courses c
                        ON c.product_id = p.product_id
    WHERE (od.deferred = 1
        AND (SELECT SUM(Payments.payment_amount)
             FROM Payments
             WHERE Payments.order_id = od.order_id
               AND p.product_id = od.product_id) >= od.price * 0.5)
       OR (od.order_paid = 1
        AND (DATEADD(DAY, -3, od.payment_date) <= p.start_date)
        AND (od.payment_date <= od.due_date))
go
```

#### 11. List of Course Modules and Students Attending Them

```sql
CREATE VIEW GetStudentsInCourseModules
AS
SELECT DISTINCT
    s.users_id,
    u.firstname,
    u.lastname,
    od.product_id,
    s.student_id
FROM Students s
INNER JOIN Users u
    ON s.users_id = u.user_id
INNER JOIN Orders o
    ON o.user_id = u.user_id
INNER JOIN Order_Details od
    ON o.order_id = od.order_id
INNER JOIN Products p
    ON p.product_id = od.product_id
INNER JOIN Course_Modules c
    ON c.product_id = p.product_id
WHERE
    (od.order_paid = 1
     AND
    od.payment_date <= od.due_date)
UNION
SELECT sic.users_id, sic.firstname, sic.lastname, cm.product_id, sic.student_id
FROM Course_Modules AS cm
INNER JOIN GetStudentsInCourses AS sic ON cm.course_id = sic.product_id
go

```

#### 12. List of Subjects and Students Attending Them

```sql
CREATE VIEW GetStudentsInSubjects
AS
SELECT DISTINCT
    s.users_id,
    u.firstname,
    u.lastname,
    od.product_id,
    s.student_id
FROM Students s
INNER JOIN Users u
    ON s.users_id = u.user_id
INNER JOIN Orders o
    ON o.user_id = u.user_id
INNER JOIN Order_Details od
    ON o.order_id = od.order_id
INNER JOIN Products p
    ON p.product_id = od.product_id
INNER JOIN Subjects sbj
    ON sbj.product_id = p.product_id
WHERE
    (od.order_paid = 1
     AND
    od.payment_date <= od.due_date)
UNION
SELECT sis.users_id, sis.firstname, sis.lastname, s.product_id, sis.student_id
FROM Subjects AS s
INNER JOIN GetStudentsInStudies AS sis ON s.study_id = sis.product_id
go
```

#### 13. List of Studies and Students Attending Them

```sql
CREATE VIEW GetStudentsInStudies
AS
SELECT DISTINCT
    s.users_id,
    u.firstname,
    u.lastname,
    od.product_id,
    s.student_id
FROM Students s
INNER JOIN Users u
    ON s.users_id = u.user_id
INNER JOIN Orders o
    ON o.user_id = u.user_id
INNER JOIN Order_Details od
    ON o.order_id = od.order_id
INNER JOIN Products p
    ON p.product_id = od.product_id
INNER JOIN Studies st
    ON st.product_id = p.product_id
WHERE
    od.deferred = 1
    OR
    (od.order_paid = 1
     AND (DATEADD(DAY, -3, od.payment_date) <= p.start_date)
     AND (od.payment_date <= od.due_date))
go
```

#### 14. Attendance Lists for Courses

```sql
CREATE VIEW Course_Module_Attendance_List AS
	WITH Course_Students AS (
		SELECT s.student_id, gsi.firstname, gsi.lastname
		FROM Students s
		INNER JOIN GetStudentsInCourseModules gsi ON gsi.users_id = s.users_id
	)
	SELECT
		cm.product_id,
		mm.meeting_id,
		mm.meeting_start_date,
		cs.student_id,
        cs.firstname,
		cs.lastname,
		CASE
			WHEN mp.student_id IS NOT NULL THEN 'Present'
			ELSE 'Absent'
		END AS attendance_status
	FROM Course_Modules cm
	JOIN Module_Meetings mm ON cm.product_id = mm.course_module_id
	JOIN Meetings_Presence mp ON mm.meeting_id = mp.meeting_id
	LEFT JOIN Course_Students cs ON cs.student_id = mp.student_id
	;
```

#### 15. Attendance Lists for Subjects

```sql
CREATE VIEW Subjects_Attendance_List AS
	WITH Subjects_Students AS (
		SELECT s.student_id, gsi.firstname, gsi.lastname
		FROM Students s
		INNER JOIN GetStudentsInSubjects gsi ON gsi.users_id = s.users_id
	)
	SELECT
		s.product_id,
		sm.meeting_id,
		sm.meeting_start_date,
        ss.student_id,
		ss.firstname,
		ss.lastname,
		CASE
			WHEN mp.student_id IS NOT NULL THEN 'Present'
			ELSE 'Absent'
		END AS attendance_status
	FROM Subjects s
	JOIN Subject_Meetings sm ON s.product_id = sm.subject_id
	JOIN Meetings_Presence mp ON sm.meeting_id = mp.meeting_id
	LEFT JOIN Subjects_Students ss ON ss.student_id = mp.student_id
	;
```

#### 16. Attendance Lists for Internships

```sql
CREATE VIEW Internship_Attendance_List AS
	SELECT
		i.internship_id,
		stu.student_id,
	CASE
		WHEN mp.student_id IS NOT NULL THEN 'Present'
		ELSE 'Absent'
	END AS attendance_status
	FROM Internship i
	JOIN Internship_Meetings im ON i.internship_id = im.internship_id
	JOIN Meetings_Presence mp ON im.meeting_id = mp.meeting_id
	LEFT JOIN Students stu ON stu.student_id = mp.student_id
	;
```

#### 17. Bilocation Report (Students Registered for Overlapping Events)

```sql
CREATE VIEW Bilocation AS
    WITH Meetings AS (
        SELECT
            o.user_id,
            sm.meeting_id,
            sm.subject_id AS meeting_course_id,
            sm.meeting_start_date,
            sm.meeting_end_date
        FROM Subject_Meetings sm
        JOIN Subjects s ON s.product_id = sm.subject_id
        JOIN Products p ON p.product_id = s.product_id
        JOIN Order_Details od ON od.product_id = p.product_id
        JOIN Orders o ON o.order_id = od.order_id
    )
    SELECT
        m1.meeting_id AS meeting_id_1,
        m2.meeting_id AS meeting_id_2,
        m1.user_id AS user_id_1,
        m1.meeting_start_date AS meeting_1_start_date,
        m2.meeting_start_date AS meeting_2_start_date,
        m1.meeting_start_date AS meeting_1_end_date,
        m2.meeting_end_date AS meeting_2_end_date
    FROM Meetings m1
    JOIN Meetings m2 ON m1.meeting_id != m2.meeting_id
        WHERE
        m1.meeting_end_date > m2.meeting_start_date
        AND m1.meeting_start_date < m2.meeting_end_date
        AND m1.meeting_id < m2.meeting_id
    --     AND m1.meeting_start_date > GETDATE()
```
