# Database Project

## Project Description
The project focuses on a database management system for a school, covering user management, courses, studies, internships, webinars, and payments. The system enables different user roles (e.g., administrator, school director, coordinator, teacher, translator, student) to perform specific actions such as managing data, generating reports, enrolling in events, scheduling, and more. DataBase was created using **DataGrip**.

## User Roles and Permissions
- **Administrator** – Manages user data, courses, and webinars.  
- **School Director** – Generates reports, postpones payments.  
- **School Coordinator** – Manages study schedules, creates syllabi, assigns grades.  
- **Teacher** – Records attendance, creates webinars.  
- **Translator** – Participates in classes, provides live translation.  
- **Internship Supervisor** – Manages internships, records attendance.  
- **Student** – Enrolls in events, manages their schedule.  
- **Unregistered User** – Views courses and webinars, registers an account.  

## System Features
- Attendance management for internships, courses, and studies.  
- Report generation (attendance, schedule conflicts, attendance lists, revenue).  
- User cart management.  
- Payment timeliness verification.  
- Participant limit control.  

## Database Diagram
The database diagram illustrates relationships between tables such as `Users`, `Students`, `Courses`, `Webinars`, `Orders`, `Payments`, and others.  
The diagram was generated using **Vertabelo**

![Database Diagram](https://github.com/alargh/SQL-Data-Base-Project/blob/main/diagram.png)

## Views
The system includes several views to facilitate report generation, such as:  
- List of debtors.  
- Number of people registered for upcoming events.  
- Attendance tracking for internships, courses, and studies.  
- List of available products.  
- Revenue summary from webinars, courses, and studies.  

## Functions and Procedures
The system includes functions and procedures for:  
- Displaying student attendance.  
- Adding new users, products, meetings, and translators.  
- Managing the user cart.  
- Processing cart orders.  
- Updating order details.  
- Adding and removing attendance records.  

## Triggers
Triggers are used to automatically verify whether an order has been fully paid and update its status accordingly.

## Test Data Generation
The project includes scripts for generating test data for tables such as `Countries`, `Languages`, `Users`, `Products`, and `Translator_Languages`.

## Indexes
Indexes have been created for all primary and foreign keys to improve query performance.
