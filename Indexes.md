# Indexes

Indexes for all primary keys are automatically created when the tables are created. Additionally, indexes are created for all foreign keys.

```sql
CREATE INDEX Coordinators_user_id on Coordinators (users_id ASC);
CREATE INDEX Course_Modules_course_id on Course_Modules (course_id ASC);
CREATE INDEX Course_Modules_coordinator_id on Course_Modules (coordinator_id ASC);
CREATE INDEX Courses_coordinator_id on Courses (coordinator_id ASC);
CREATE INDEX Internship_study_id on Internship (study_id ASC);
CREATE INDEX Internship_Meetings_internship_id on Internship_Meetings (internship_id ASC);
CREATE INDEX Module_Meetings_course_module_id on Module_Meetings (course_module_id ASC);
CREATE INDEX Order_user_id on Orders (user_id ASC);
CREATE INDEX Payments_order_id on Payments (order_id ASC);
CREATE INDEX Payments_product_id on Payments (product_id ASC);
CREATE INDEX Student_User_Id on Students (users_id ASC);
CREATE INDEX Studies_coordinator_id on Studies (coordinator_id ASC);
CREATE INDEX Subject_Meetings_subject_id on Subject_Meetings (subject_id ASC);
CREATE INDEX Subjects_study_id on Subjects (study_id ASC);
CREATE INDEX Subjects_coordinator_id on Subjects (coordinator_id ASC);
CREATE INDEX Translator_user_id on Translators (user_id ASC);
CREATE INDEX Tutor_user_id on Tutors (user_id ASC);
CREATE INDEX User_country on Users (country ASC);
CREATE INDEX Webinar_tutor_id on Webinars (tutor_id ASC);
CREATE INDEX Webinar_translator_id on Webinars (translator_id ASC);
CREATE INDEX Webinar_meeting_id on Webinars (meeting_id ASC);
```
