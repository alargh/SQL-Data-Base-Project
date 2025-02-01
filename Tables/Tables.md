# Table Descriptions (Iwo Zowada)

## Users Category

### Users Table

Contains basic information about each user.

- **user_id** int - primary key, user identifier
- firstname varchar(100) - first name
- lastname varchar(100) - last name
- email varchar(100) - email address
- phone varchar(20) - phone number
- country varchar(100) - country of origin
- postal_code varchar(100) - postal code
- city varchar(100) - city
- street varchar(100) - street
- apartment_no int - apartment number
- birth_date date - date of birth

### Students Table

Contains information about students.

- **student_id** int - klucz główny, identyfikator studenta
- user_id int - klucz obcy, identyfikator użytkownika

```sql
-- Table: Students
CREATE TABLE Students (
    student_id int  NOT NULL IDENTITY,
    users_id int  NOT NULL,
    CONSTRAINT Students_pk PRIMARY KEY  (student_id)
);
```

- **student_id** int - primary key, student identifier
- user_id int - foreign key, user identifier

### Coordinators Table

Contains information about coordinators.

```sql
-- Table: Coordinators
CREATE TABLE Coordinators (
    coordinator_id int  NOT NULL IDENTITY,
    users_id int  NOT NULL,
    CONSTRAINT Coordinators_pk PRIMARY KEY  (coordinator_id)
);
```

- **coordinator_id** int - primary key, coordinator identifier
- user_id int - foreign key, user identifier

### Countries Table

Contains information about countries.

```sql
-- Table: Countries
CREATE TABLE Countries (
    country_id int  NOT NULL IDENTITY,
    country_label varchar(100)  NOT NULL,
    CONSTRAINT UNIQUE_COUNTRY UNIQUE (country_label),
    CONSTRAINT Countries_pk PRIMARY KEY  (country_id)
);
```

- **country_id** int - primary key, country identifier
- country_label varchar(100) - country name

### Translators Category

### Translators Table

Contains information about translators.

```sql
-- Table: Translator_Languages
CREATE TABLE Translator_Languages (
    translator_id int  NOT NULL,
    language_id int  NOT NULL,
    CONSTRAINT Translator_Languages_pk PRIMARY KEY  (translator_id,language_id)
);
```

- **translator_id** int - primary key, translator identifier
- user_id int - foreign key, user identifier for the translator

### Translator_Languages Table

Contains information about which languages each translator teaches.

```sql
-- Table: Languages
CREATE TABLE Languages (
    language_id int  NOT NULL,
    language_label varchar(100)  NOT NULL,
    CONSTRAINT UNIQUE_LANGUAGE UNIQUE (language_label),
    CONSTRAINT Languages_pk PRIMARY KEY  (language_id)
);
```

- translator_id int - foreign key, translator identifier
- language_id int - foreign key, language identifier

### Languages Table

Contains information about languages and their identifiers.

- **language_id** int - primary key, language identifier
- language_label varchar(100) - name of the language assigned to the identifier

### Orders Category

### Orders Table

Contains information about all orders (paid or unpaid), with the following details:

```sql
-- Table: Orders
CREATE TABLE Orders (
    order_id int  NOT NULL IDENTITY,
    user_id int  NOT NULL,
    order_date datetime  NOT NULL,
    payment_url varchar(100)  NOT NULL,
    payment_date datetime  NOT NULL,
    due_date datetime  NOT NULL,
    CONSTRAINT Orders_pk PRIMARY KEY  (order_id)
);
```

- **order_id** int - primary key, order identifier
- user_id int - foreign key, user identifier who placed the order
- order_date datetime - order placement date and time
- payment_url varchar(100) - payment link for the user
- payment_date datetime - date of payment for the order
- due_date datetime - due date for the payment

### Order_Details Table

Contains detailed information about a specific product in an order.

```sql
-- Table: Order_Details
CREATE TABLE Order_Details (
    order_id int  NOT NULL,
    product_id int  NOT NULL,
    price money  NOT NULL,
    deferred bit  NOT NULL,
    deferred_date datetime NULL,
    order_paid bit NOT NULL,
    CONSTRAINT Order_Details_pk PRIMARY KEY  (order_id,product_id)
);
```

- order_id int - foreign key, order identifier
- product_id int - foreign key, product identifier
- price money - price of the product for this order (can change due to discounts from the director)
- deferred bit - whether the order is deferred
- deferred_date datetime - deferred date for the order
- order_paid bit - whether the order is paid

### Products Table

Contains information about products.

```sql
-- Table: Products
CREATE TABLE Products (
    product_id int  NOT NULL IDENTITY,
    product_price money  NOT NULL,
    product_title varchar(100)  NOT NULL,
    product_description varchar(max)  NOT NULL,
    release_date datetime  NOT NULL,
    start_date datetime  NOT NULL,
    to_buy bit NOT NULL DEFAULT 1,
    CONSTRAINT Products_Price CHECK (0 <= product_price AND product_price <= 10000),
    CONSTRAINT Products_Release_start_date CHECK (release_date < start_date),
    CONSTRAINT Products_pk PRIMARY KEY  (product_id)
);
```

- **product_id** int - primary key, product identifier
- product_price money - standard price of the product (between 0 and 10,000)
- product_title varchar(100) - product name
- product_description varchar(max) - product description
- release_date datetime - date when the product will be available for purchase (release date must be before start date)
- start_date datetime - course start date (e.g., when the course begins)
- to_buy bit - whether the product can be purchased (for blocking products that are not for sale, e.g., webinars as part of a course/study)

### Shopping_Cart Table

Contains information about the products a user intends to buy.

```sql
-- Table: Shopping_Cart
CREATE TABLE Shopping_Cart (
    user_id int  NOT NULL,
    product_id int  NOT NULL,
    CONSTRAINT Shopping_Cart_pk PRIMARY KEY  (user_id,product_id)
);
```

- user_id int - foreign key, user identifier
- product_id int - foreign key, product identifier

### Payments Table

Contains information about payments for orders.

```sql
-- Table: Payments
CREATE TABLE Payments (
    payment_id int  NOT NULL IDENTITY,
    order_id int  NOT NULL,
    product_id int  NOT NULL,
    payment_date datetime  NOT NULL,
    payment_amount money  NOT NULL,
    CONSTRAINT Payments_pk PRIMARY KEY  (payment_id)
);
```

- payment_id int - primary key, payment identifier
- order_id int - foreign key, order identifier
- product_id int - foreign key, product identifier
- payment_date datetime - payment date
- payment_amount money - amount paid for the order

### Webinars Category

### Tutors Table

Contains information about which users are teachers.

```sql
-- Table: Tutors
CREATE TABLE Tutors (
    tutor_id int  NOT NULL IDENTITY,
    user_id int  NOT NULL,
    CONSTRAINT Tutors_pk PRIMARY KEY  (tutor_id)
);
```

- **tutor_id** int - primary key, teacher identifier
- user_id int - foreign key, user identifier

### Webinars Table

Contains information about available webinars.

```sql
-- Table: Webinars
CREATE TABLE Webinars (
    product_id int  NOT NULL,
    tutor_id int  NOT NULL,
    translator_id int  NOT NULL,
    webinar_date datetime  NOT NULL,
    record_start_date datetime  NOT NULL,
    record_end_date datetime  NOT NULL,
    record_url varchar(100)  NOT NULL,
    meeting_id int NULL DEFAULT NULL,
    CONSTRAINT Webinars_pk PRIMARY KEY  (product_id)
);
```

- product_id int - foreign key, product identifier
- tutor_id int - foreign key, tutor identifier for the webinar
- translator_id int - foreign key, translator identifier who translates the webinar into Polish
- webinar_date datetime - date of the webinar
- record_start_date datetime - date when the recording will be available to users
- record_end_date datetime - date when the recording will no longer be available (30 days after the start of the recording access)
- meeting_id int - foreign key, meeting identifier if the webinar is part of a course/study program

### Studies Category

### Studies Table

Contains information about a specific study program.

```sql
-- Table: Studies
CREATE TABLE Studies (
    product_id int  NOT NULL,
    place_limits int  NOT NULL,
    coordinator_id int  NOT NULL,
    CONSTRAINT Studies_Place_limits CHECK (0 < place_limits AND place_limits < 1000),
    CONSTRAINT Studies_pk PRIMARY KEY  (product_id)
);
```

- **product_id** int - primary and foreign key, product identifier, each product in this table is a different study program
- place_limits int - number of places available for the program (between 1 and 999)
- coordinator_id - foreign key, coordinator identifier for the study program

### Subjects Table

Contains information about subjects within a study program (these can also be purchased as separate products).

```sql
-- Table: Subjects
CREATE TABLE Subjects (
    product_id int  NOT NULL,
    study_id int  NOT NULL,
    coordinator_id int  NOT NULL,
    place_limits int  NOT NULL,
    CONSTRAINT Subjects_Place_limits CHECK (0 < place_limits AND place_limits < 1000),
    CONSTRAINT Subjects_pk PRIMARY KEY  (product_id)
);
```

- **product_id** int - primary and foreign key, product identifier, each product in this table is a different subject in the study program
- study_id int - foreign key, study program identifier
- coordinator_id int - foreign key, coordinator identifier for the subject
- place_limits int - number of places available for the subject

### Internship Table

Contains information about internships.

```sql
-- Table: Internship
CREATE TABLE Internship (
    internship_id int  NOT NULL,
    study_id int  NOT NULL,
    start_date datetime  NOT NULL,
    end_date datetime  NOT NULL,
    CONSTRAINT Internship_Start_end_date CHECK (start_date < end_date),
    CONSTRAINT Internship_pk PRIMARY KEY  (internship_id)
);
```

- **internship_id** int - primary key, internship identifier
- study_id int - foreign key, study program identifier
- start_date datetime - internship start date
- end_date datetime - internship end date (14 days after start)

### Courses Category

### Courses Table

Contains information about all available courses.

```sql
-- Table: Courses
CREATE TABLE Courses (
    product_id int  NOT NULL,
    place_limits int  NOT NULL,
    coordinator_id int  NOT NULL,
    CONSTRAINT Courses_Place_limits CHECK (0 < place_limits AND place_limits < 1000),
    CONSTRAINT Courses_pk PRIMARY KEY  (product_id)
);
```

- **product_id** int - primary and foreign key, product identifier, each product in this table is a different course
- place_limits int - number of places available for the course (between 1 and 999)
- coordinator_id - foreign key, coordinator identifier for the course

### Course_Modules Table

Contains information about subjects within a course (these can also be purchased as separate products).

```sql
-- Table: Course_Modules
CREATE TABLE Course_Modules (
    product_id int  NOT NULL,
    course_id int  NOT NULL,
    coordinator_id int  NOT NULL,
    place_limits int  NOT NULL,
    CONSTRAINT Course_Modules_Place_limits CHECK (0 < place_limits AND place_limits < 1000),
    CONSTRAINT Course_Modules_pk PRIMARY KEY  (product_id)
);
```

- **product_id** int - primary and foreign key, product identifier, each product in this table is a different module of the course
- course_id int - foreign key, course identifier
- coordinator_id int - foreign key, coordinator identifier for the module
- place_limits int - number of places available for the module

### Meetings Category

### Meetings Table

Contains information about all meetings.

```sql
-- Table: Meetings
CREATE TABLE Meetings (
    meeting_id int  NOT NULL IDENTITY,
    CONSTRAINT Meetings_pk PRIMARY KEY  (meeting_id)
);
```

- **meeting_id** int - primary key, meeting identifier

### Internship_Meetings Table

Contains information about meetings related to internships.

```sql
-- Table: Internship_Meetings
CREATE TABLE Internship_Meetings (
    meeting_id int  NOT NULL,
    internship_id int  NOT NULL,
    CONSTRAINT Internship_Meetings_pk PRIMARY KEY  (meeting_id)
);
```

- **meeting_id** int - primary key, meeting identifier
- internship_id int - foreign key, internship identifier

### Subject_Meetings Table

Contains information about meetings for different subjects.

```sql
-- Table: Subject_Meetings
CREATE TABLE Subject_Meetings (
    meeting_id int  NOT NULL,
    subject_id int  NOT NULL,
    meeting_type varchar(100)  NOT NULL,
    classroom varchar(100) NULL DEFAULT NULL,
    CONSTRAINT Subject_Meetings_Meeting_type CHECK (meeting_type IN ('Stationary', 'OnlineSync', 'OnlineAsync', 'Session')),
    CONSTRAINT Subject_Meetings_Meeting_date CHECK (meeting_start_date < meeting_end_date),
    CONSTRAINT Subject_Meetings_pk PRIMARY KEY  (meeting_id)
);
```

- **meeting_id** int - primary key, meeting identifier
- subject_id int - foreign key, subject identifier
- meeting_type varchar(100) - type of the meeting (e.g., stationary, online synchronous, online asynchronous, hybrid for different teaching methods)

### Module_Meetings Table

Contains information about meetings for different course modules.

```sql
-- Table: Module_Meetings
CREATE TABLE Module_Meetings (
    meeting_id int  NOT NULL,
    course_module_id int  NOT NULL,
    meeting_type varchar(100)  NOT NULL,
    classroom varchar(100) NULL DEFAULT NULL,
    CONSTRAINT Module_Meetings_Meeting_type CHECK (meeting_type IN ('Stationary', 'OnlineSync', 'OnlineAsync')),
    CONSTRAINT Module_Meetings_Meeting_date CHECK (meeting_start_date < meeting_end_date),
    CONSTRAINT Module_Meetings_pk PRIMARY KEY  (meeting_id)
);
```

- **meeting_id** int - primary key, meeting identifier
- course_module_id int - foreign key, course module identifier
- meeting_type varchar(100) - type of the meeting (e.g., stationary, online synchronous, online asynchronous)

### Meetings_Presence Table

Contains information about which students attended which meetings.

```sql
-- Table: Meetings_Presence
CREATE TABLE Meetings_Presence (
    student_id int  NOT NULL,
    meeting_id int  NOT NULL
);
```

- student_id int - foreign key, student identifier
- meeting_id int - foreign key, meeting identifier

### Table Relationships (Generated by Vertabelo)

```sql
-- foreign keys
-- Reference: Coordinators_Users (table: Coordinators)
ALTER TABLE Coordinators ADD CONSTRAINT Coordinators_Users
    FOREIGN KEY (users_id)
    REFERENCES Users (user_id);

-- Reference: Course_Modules_Coordinators (table: Course_Modules)
ALTER TABLE Course_Modules ADD CONSTRAINT Course_Modules_Coordinators
    FOREIGN KEY (coordinator_id)
    REFERENCES Coordinators (coordinator_id);

-- Reference: Course_Modules_Courses (table: Course_Modules)
ALTER TABLE Course_Modules ADD CONSTRAINT Course_Modules_Courses
    FOREIGN KEY (course_id)
    REFERENCES Courses (product_id);

-- Reference: Course_Modules_Products (table: Course_Modules)
ALTER TABLE Course_Modules ADD CONSTRAINT Course_Modules_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Courses_Coordinators (table: Courses)
ALTER TABLE Courses ADD CONSTRAINT Courses_Coordinators
    FOREIGN KEY (coordinator_id)
    REFERENCES Coordinators (coordinator_id);

-- Reference: Courses_Products (table: Courses)
ALTER TABLE Courses ADD CONSTRAINT Courses_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Employees_Users (table: Tutors)
ALTER TABLE Tutors ADD CONSTRAINT Employees_Users
    FOREIGN KEY (user_id)
    REFERENCES Users (user_id);

-- Reference: Internship_Meetings_Internship (table: Internship_Meetings)
ALTER TABLE Internship_Meetings ADD CONSTRAINT Internship_Meetings_Internship
    FOREIGN KEY (internship_id)
    REFERENCES Internship (internship_id);

-- Reference: Internship_Studies (table: Internship)
ALTER TABLE Internship ADD CONSTRAINT Internship_Studies
    FOREIGN KEY (study_id)
    REFERENCES Studies (product_id);

-- Reference: Meeting_Presence_Internship_Meetings (table: Internship_Meetings)
ALTER TABLE Internship_Meetings ADD CONSTRAINT Meeting_Presence_Internship_Meetings
    FOREIGN KEY (meeting_id)
    REFERENCES Meetings (meeting_id);

-- Reference: Meeting_Presence_Module_Meetings (table: Module_Meetings)
ALTER TABLE Module_Meetings ADD CONSTRAINT Meeting_Presence_Module_Meetings
    FOREIGN KEY (meeting_id)
    REFERENCES Meetings (meeting_id);

-- Reference: Meeting_Presence_Study_Meeting (table: Subject_Meetings)
ALTER TABLE Subject_Meetings ADD CONSTRAINT Meeting_Presence_Study_Meeting
    FOREIGN KEY (meeting_id)
    REFERENCES Meetings (meeting_id);

-- Reference: Meetings_Presence_Meetings (table: Meetings_Presence)
ALTER TABLE Meetings_Presence ADD CONSTRAINT Meetings_Presence_Meetings
    FOREIGN KEY (meeting_id)
    REFERENCES Meetings (meeting_id);

-- Reference: Meetings_Presence_Students (table: Meetings_Presence)
ALTER TABLE Meetings_Presence ADD CONSTRAINT Meetings_Presence_Students
    FOREIGN KEY (student_id)
    REFERENCES Students (student_id);

-- Reference: Module_Meetings_Course_Modules (table: Module_Meetings)
ALTER TABLE Module_Meetings ADD CONSTRAINT Module_Meetings_Course_Modules
    FOREIGN KEY (course_module_id)
    REFERENCES Course_Modules (product_id);

-- Reference: Order_Details_Orders (table: Order_Details)
ALTER TABLE Order_Details ADD CONSTRAINT Order_Details_Orders
    FOREIGN KEY (order_id)
    REFERENCES Orders (order_id);

-- Reference: Order_Details_Products (table: Order_Details)
ALTER TABLE Order_Details ADD CONSTRAINT Order_Details_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Orders_Users (table: Orders)
ALTER TABLE Orders ADD CONSTRAINT Orders_Users
    FOREIGN KEY (user_id)
    REFERENCES Users (user_id);

-- Reference: Payments_Order_Details (table: Payments)
ALTER TABLE Payments ADD CONSTRAINT Payments_Order_Details
    FOREIGN KEY (order_id,product_id)
    REFERENCES Order_Details (order_id,product_id);

-- Reference: Shopping_Cart_Products (table: Shopping_Cart)
ALTER TABLE Shopping_Cart ADD CONSTRAINT Shopping_Cart_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Shopping_Cart_Users (table: Shopping_Cart)
ALTER TABLE Shopping_Cart ADD CONSTRAINT Shopping_Cart_Users
    FOREIGN KEY (user_id)
    REFERENCES Users (user_id);

-- Reference: Students_Users (table: Students)
ALTER TABLE Students ADD CONSTRAINT Students_Users
    FOREIGN KEY (users_id)
    REFERENCES Users (user_id);

-- Reference: Studies_Coordinators (table: Studies)
ALTER TABLE Studies ADD CONSTRAINT Studies_Coordinators
    FOREIGN KEY (coordinator_id)
    REFERENCES Coordinators (coordinator_id);

-- Reference: Studies_Products (table: Studies)
ALTER TABLE Studies ADD CONSTRAINT Studies_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Study_Meeting_Subject (table: Subject_Meetings)
ALTER TABLE Subject_Meetings ADD CONSTRAINT Study_Meeting_Subject
    FOREIGN KEY (subject_id)
    REFERENCES Subjects (product_id);

-- Reference: Subject_Coordinators (table: Subjects)
ALTER TABLE Subjects ADD CONSTRAINT Subject_Coordinators
    FOREIGN KEY (coordinator_id)
    REFERENCES Coordinators (coordinator_id);

-- Reference: Subject_Products (table: Subjects)
ALTER TABLE Subjects ADD CONSTRAINT Subject_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Subject_Studies (table: Subjects)
ALTER TABLE Subjects ADD CONSTRAINT Subject_Studies
    FOREIGN KEY (study_id)
    REFERENCES Studies (product_id);

-- Reference: Translator_Languages_Languages (table: Translator_Languages)
ALTER TABLE Translator_Languages ADD CONSTRAINT Translator_Languages_Languages
    FOREIGN KEY (language_id)
    REFERENCES Languages (language_id);

-- Reference: Translator_Languages_Translators (table: Translator_Languages)
ALTER TABLE Translator_Languages ADD CONSTRAINT Translator_Languages_Translators
    FOREIGN KEY (translator_id)
    REFERENCES Translators (translator_id);

-- Reference: Translators_Users (table: Translators)
ALTER TABLE Translators ADD CONSTRAINT Translators_Users
    FOREIGN KEY (user_id)
    REFERENCES Users (user_id);

-- Reference: Users_Countries (table: Users)
ALTER TABLE Users ADD CONSTRAINT Users_Countries
    FOREIGN KEY (country)
    REFERENCES Countries (country_id);

-- Reference: Webinars_Meetings (table: Webinars)
ALTER TABLE Webinars ADD CONSTRAINT Webinars_Meetings
    FOREIGN KEY (meeting_id)
    REFERENCES Meetings (meeting_id);

-- Reference: Webinars_Products (table: Webinars)
ALTER TABLE Webinars ADD CONSTRAINT Webinars_Products
    FOREIGN KEY (product_id)
    REFERENCES Products (product_id);

-- Reference: Webinars_Translators (table: Webinars)
ALTER TABLE Webinars ADD CONSTRAINT Webinars_Translators
    FOREIGN KEY (translator_id)
    REFERENCES Translators (translator_id);

-- Reference: Webinars_Tutors (table: Webinars)
ALTER TABLE Webinars ADD CONSTRAINT Webinars_Tutors
    FOREIGN KEY (tutor_id)
    REFERENCES Tutors (tutor_id);

-- End of file.
```
