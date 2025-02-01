# Procedures

#### 1. Adds a meeting based on its type (internship, subject, or module) and associates it with other data such as the date, classroom, or online sync details.


```sql
CREATE PROCEDURE Add_Meeting_With_Type
    @meeting_type VARCHAR(50), -- 'internship', 'subject', 'module'
    @related_id INT, -- internship_id, subject_id, or course_module_id
    @type_of_meeting VARCHAR(100) = NULL,
    @meeting_classroom VARCHAR(100) = NULL,
    @meeting_start_date DATETIME = NULL,
    @meeting_end_date DATETIME = NULL,
    -- Webinar
    @tutor_id INT = NULL,
    @translator_id INT = NULL,
    @record_url VARCHAR(100) = NULL
AS
BEGIN
    DECLARE @new_meeting_id INT;
    BEGIN TRY
        BEGIN TRANSACTION;
        IF @meeting_type NOT IN ('internship', 'subject', 'module')
            BEGIN
                RAISERROR('Nieprawidłowy typ spotkania.', 16, 1);
                RETURN;
            END
        INSERT INTO Meetings DEFAULT VALUES;
        SET @new_meeting_id = SCOPE_IDENTITY();

        -- Based on meeting type, insert into related tables
        IF @meeting_type = 'internship'
            BEGIN
                INSERT INTO Internship_Meetings (meeting_id, internship_id)
                VALUES (@new_meeting_id, @related_id);
            END
        ELSE IF @meeting_type = 'subject'
            BEGIN
                IF @meeting_start_date IS NULL OR @meeting_end_date IS NULL
                    BEGIN
                        RAISERROR('Nie podano daty rozpoczęcia lub zakończenia spotkania.', 16, 1);
                        RETURN;
                    END
                IF @type_of_meeting = 'Stationary'
                    BEGIN
                        IF @meeting_classroom IS NULL
                            BEGIN
                                RAISERROR('Nie podano sali spotkania.', 16, 1);
                                RETURN;
                            END
                        INSERT INTO Subject_Meetings (meeting_id, subject_id, meeting_type, classroom, meeting_start_date, meeting_end_date)
                        VALUES (@new_meeting_id, @related_id, @type_of_meeting, @meeting_classroom, @meeting_start_date, @meeting_end_date);
                    END
                ELSE
                    BEGIN
                        IF @type_of_meeting = 'OnlineSync'
                            BEGIN
                                IF @tutor_id IS NULL OR @translator_id IS NULL OR @record_url IS NULL
                                    BEGIN
                                        RAISERROR('Nie podano wszystkich wymaganych danych.', 16, 1);
                                        RETURN;
                                    END
                                PRINT @meeting_start_date;
                                EXEC Add_Product_With_Type
                                     0,
                                     'Online Sync Meeting',
                                     'Subject Online Sync Meeting',
                                     @meeting_start_date,
                                     @meeting_end_date,
                                     @product_type='Webinar',
                                     @to_buy=0,
                                     @tutor_id=@tutor_id,
                                     @translator_id=@translator_id,
                                     @webinar_date=@meeting_start_date,
                                     @record_start_date=@meeting_start_date,
                                     @record_end_date=@meeting_end_date,
                                     @record_url=@record_url,
                                     @meeting_id=@new_meeting_id;
                            END
                        INSERT INTO Subject_Meetings (meeting_id, subject_id, meeting_type, meeting_start_date, meeting_end_date)
                        VALUES (@new_meeting_id, @related_id, @type_of_meeting, @meeting_start_date, @meeting_end_date);
                    END
            END
        ELSE IF @meeting_type = 'module'
            BEGIN
                IF @meeting_start_date IS NULL OR @meeting_end_date IS NULL
                    BEGIN
                        RAISERROR('Nie podano daty rozpoczęcia lub zakończenia spotkania.', 16, 1);
                        RETURN;
                    END
                IF @type_of_meeting = 'Stationary'
                    BEGIN
                        IF @meeting_classroom IS NULL
                            BEGIN
                                RAISERROR('Nie podano sali spotkania.', 16, 1);
                                RETURN;
                            END
                        INSERT INTO Module_Meetings (meeting_id, course_module_id, meeting_type, classroom, meeting_start_date, meeting_end_date)
                        VALUES (@new_meeting_id, @related_id, @type_of_meeting, @meeting_classroom, @meeting_start_date, @meeting_end_date);
                    END
                ELSE
                    BEGIN
                        IF @type_of_meeting = 'OnlineSync'
                        BEGIN
                            IF @tutor_id IS NULL OR @translator_id IS NULL OR @record_url IS NULL
                                BEGIN
                                    RAISERROR('Nie podano wszystkich wymaganych danych.', 16, 1);
                                    RETURN;
                                END
                            EXEC Add_Product_With_Type
                                 0,
                                 'Online Sync Meeting',
                                 'Module Online Sync Meeting',
                                 @meeting_start_date,
                                 @meeting_end_date,
                                 @product_type='Webinar',
                                 @to_buy=0,
                                 @tutor_id=@tutor_id,
                                 @translator_id=@translator_id,
                                 @webinar_date=@meeting_start_date,
                                 @record_start_date=@meeting_start_date,
                                 @record_end_date=@meeting_end_date,
                                 @record_url=@record_url,
                                 @meeting_id=@new_meeting_id;
                        END
                        INSERT INTO Module_Meetings (meeting_id, course_module_id, meeting_type, meeting_start_date, meeting_end_date)
                        VALUES (@new_meeting_id, @related_id, @type_of_meeting, @meeting_start_date, @meeting_end_date);
                    END
            END
        PRINT 'Spotkanie zostało pomyślnie dodane do odpowiednich tabel.';
        COMMIT;
    END TRY
    BEGIN CATCH
        THROW;
    END CATCH
END;
go
```

#### 2. Adds a product (such as a course, study, module, or webinar) and stores related details in the respective tables.

```sql
CREATE PROCEDURE Add_Product_With_Type
    -- Products
    @product_price MONEY,
    @product_title VARCHAR(100),
    @product_description VARCHAR(MAX),
    @release_date DATETIME,
    @start_date DATETIME,
    @to_buy BIT = 1,
    -- Product Type
    @product_type VARCHAR(50), -- Typ produktu: 'Course', 'Study', 'Module', 'Subject', 'Webinar'

-- Other ID
    @course_id INT = NULL, -- ID kursu, studium itp.
    @study_id INT = NULL, -- ID studium
    @tutor_id INT = NULL, -- ID tutora
    @translator_id INT = NULL, -- ID tłumacza
    @coordinator_id INT = NULL, -- ID koordynatora
-- Additional
    @place_limits INT = NULL,  -- Limit miejsc dla kursu, studium itp.
    @webinar_date DATETIME = NULL, -- Data webinaru (jeśli produkt to webinar)
    @record_start_date DATETIME = NULL,
    @record_end_date DATETIME = NULL,
    @record_url VARCHAR(100) = NULL,
    @meeting_id INT = NULL
AS
BEGIN
    DECLARE @new_product_id INT;
    PRINT 'Dodawanie nowego produktu do bazy danych...';
    BEGIN TRY
        BEGIN TRANSACTION;

        -- Dodanie nowego produktu do tabeli Products
        INSERT INTO Products (
            product_price,
            product_title,
            product_description,
            release_date,
            start_date,
            to_buy
        )
        VALUES (
                   @product_price,
                   @product_title,
                   @product_description,
                   @release_date,
                   @start_date,
                     @to_buy
               );

        SET @new_product_id = SCOPE_IDENTITY();

        -- Dodanie do odpowiedniej tabeli w zależności od typu produktu
        IF @product_type = 'Course'
            BEGIN
                INSERT INTO Courses (product_id, place_limits, coordinator_id)
                VALUES (@new_product_id, @place_limits, @coordinator_id);
            END

        ELSE IF @product_type = 'Study'
            BEGIN
                INSERT INTO Studies (product_id, place_limits, coordinator_id)
                VALUES (@new_product_id, @place_limits, @coordinator_id);
            END

        ELSE IF @product_type = 'Module'
            BEGIN
                INSERT INTO Course_Modules (product_id, course_id, coordinator_id, place_limits)
                VALUES (@new_product_id, @course_id, @coordinator_id, @place_limits);
            END

        ELSE IF @product_type = 'Subject'
            BEGIN
                INSERT INTO Subjects (product_id, study_id, coordinator_id, place_limits)
                VALUES (@new_product_id, @study_id, @coordinator_id, @place_limits);
            END

        ELSE IF @product_type = 'Webinar'
            BEGIN
                INSERT INTO Webinars (
                    product_id,
                    tutor_id,
                    translator_id,
                    webinar_date,
                    record_start_date,
                    record_end_date,
                    record_url,
                    meeting_id
                )
                VALUES (
                           @new_product_id,
                           @tutor_id,
                           @translator_id,
                           @webinar_date,
                           @record_start_date,
                           @record_end_date,
                           @record_url,
                            @meeting_id
                       );
            END

        PRINT 'Produkt został pomyślnie dodany do odpowiednich tabel.';
        COMMIT;
    END TRY
    BEGIN CATCH
        THROW;
    END CATCH
END;
go
```

#### 3. Assigns a language to a translator.

```sql
CREATE PROCEDURE Add_Translator_Language
    @translator_id INT,
    @language_name VARCHAR(100)
AS
BEGIN
    BEGIN TRY
        DECLARE @language_id INT;
        SELECT @language_id = language_id FROM Languages WHERE language_label = @language_name;
        IF @language_id IS NULL
        BEGIN
            THROW 50000, 'Podany język nie istnieje', 1;
        END
        IF EXISTS (SELECT * FROM Translator_Languages WHERE translator_id = @translator_id AND language_id = @language_id)
        BEGIN
            THROW 50000, 'Język jest już przypisany do tłumacza', 1;
        END
        INSERT INTO Translator_Languages (translator_id, language_id) VALUES (@translator_id, @language_id);
    END TRY
    BEGIN CATCH
        THROW;
    END CATCH
END;
go
```

#### 4. Adds a user (student, translator, coordinator, or tutor) and associates them with their relevant table.

```sql
CREATE PROCEDURE Add_User_With_Type
    @firstname VARCHAR(100),
    @lastname VARCHAR(100),
    @email VARCHAR(100),
    @phone VARCHAR(20),
    @country VARCHAR(100),
    @postal_code VARCHAR(100),
    @city VARCHAR(100),
    @street VARCHAR(100),
    @apartment_no INT,
    @birth_date DATE,
    @user_type VARCHAR(50) -- Typ użytkownika: 'Student', 'Translator', 'Coordinator', 'Tutor'
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;
        DECLARE @country_id INT;
        SET @country_id = (SELECT country_id FROM Countries WHERE country_label = @country);
        IF @country_id IS NULL
        BEGIN
            THROW 50000, 'Nie znaleziono kraju o podanej nazwie.', 1;
        END

        DECLARE @new_user_id INT;
        -- Dodanie nowego użytkownika do tabeli Users
        INSERT INTO Users (
            firstname,
            lastname,
            email,
            phone,
            country,
            postal_code,
            city,
            street,
            apartment_no,
            birth_date
        )
        VALUES (
            @firstname,
            @lastname,
            @email,
            @phone,
            @country_id,
            @postal_code,
            @city,
            @street,
            @apartment_no,
            @birth_date
        );
        SET @new_user_id = SCOPE_IDENTITY(); -- Pobranie ID nowego użytkownika

        -- Dodanie użytkownika do odpowiedniej tabeli w zależności od typu użytkownika
        IF @user_type = 'Student'
        BEGIN
            INSERT INTO Students (users_id)
            VALUES (
                @new_user_id
            );
        END

        ELSE IF @user_type = 'Translator'
        BEGIN
            INSERT INTO Translators (user_id)
            VALUES (
                @new_user_id
            );
        END

        ELSE IF @user_type = 'Coordinator'
        BEGIN
            INSERT INTO Coordinators (users_id)
            VALUES (
                @new_user_id
            );
        END

        ELSE IF @user_type = 'Tutor'
        BEGIN
            INSERT INTO Tutors (user_id)
            VALUES (
                @new_user_id
            );
        END

        PRINT 'Użytkownik został pomyślnie dodany do odpowiednich tabel.';
        COMMIT;
    END TRY
    BEGIN CATCH
        THROW;
    END CATCH
END;
go
```

#### 5. Adds a product to a user's shopping cart.

```sql
CREATE PROCEDURE Add_Product_To_Cart
    @UserId INT,
    @ProductId INT
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        IF EXISTS (
            SELECT 1
            FROM Available_Products
            WHERE product_id = @ProductId
        )
        BEGIN
            IF EXISTS (
                SELECT 1
                FROM Shopping_Cart
                WHERE user_id = @UserId AND product_id = @ProductId
            )
            BEGIN
                RAISERROR ('The product is already in the cart.', 16, 1);
                ROLLBACK TRANSACTION;
                RETURN;
            END
            ELSE
            BEGIN
                INSERT INTO Shopping_Cart (user_id, product_id)
                VALUES (@UserId, @ProductId);
            END
        END
        ELSE
        BEGIN
            RAISERROR ('The product cannot be added to the cart as it is not available.', 16, 1);
            ROLLBACK TRANSACTION;
            RETURN;
        END
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END;
go
```

#### 6. Creates an order from a user's cart and handles the payment details.

```sql
CREATE PROCEDURE Add_Order_From_Cart
    @UserID INT,
    @PaymentURL VARCHAR(100),
    @DueDate DATETIME
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Users WHERE user_id = @UserID)
        BEGIN
            THROW 50000, 'Podany użytkownik nie istnieje', 1;
        END

        DECLARE @OrderID INT;

        INSERT INTO Orders (user_id, order_date, payment_url)
        VALUES (@UserID, GETDATE(), @PaymentURL);

        SET @OrderID = SCOPE_IDENTITY();

        INSERT INTO Order_Details (order_id, product_id, price, deferred, deferred_date, payment_date, due_date)
        SELECT
            @OrderID,
            sc.product_id,
            p.product_price,
            0 AS deferred,
            NULL AS deferred_date,
            NULL AS payment_date,
            @DueDate as due_date
        FROM Shopping_Cart sc
        INNER JOIN Products p ON sc.product_id = p.product_id
	INNER JOIN Available_Products ap WITH (UPDLOCK) ON ap.product_id = sc.product_id
        WHERE sc.user_id = @UserID;

        DELETE FROM Shopping_Cart
        WHERE user_id = @UserID;

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        -- Re-throw the error
        THROW;
    END CATCH
END;
go
```

#### 7. Updates specific details of an order, such as price or deferred payment.

```sql
CREATE PROCEDURE Update_Order_Details
    @OrderID INT,
    @ProductID INT,
    @Price MONEY = NULL,
    @Deferred BIT = NULL,
    @DeferredDate DATETIME = NULL,
    @OrderPaid BIT = NULL,
    @PaymentDate datetime = NULL,
    @DueDate datetime = NULL
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Orders WHERE order_id = @OrderID)
        BEGIN
            THROW 50000, 'Order with the given ID does not exist.', 1;
        END

        IF NOT EXISTS (SELECT 1 FROM Order_Details WHERE order_id = @OrderID AND product_id = @ProductID)
        BEGIN
            THROW 50001, 'No matching record found in Order_Details for the given OrderID and ProductID.', 1;
        END

        UPDATE Order_Details
        SET
            price = ISNULL(@Price, price),
            deferred = ISNULL(@Deferred, deferred),
            deferred_date = ISNULL(@DeferredDate, deferred_date),
            order_paid = ISNULL(@OrderPaid, order_paid),
            payment_date = ISNULL(@PaymentDate, payment_date),
            due_date = ISNULL(@DueDate, due_date)
        WHERE order_id = @OrderID AND product_id = @ProductID;

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        THROW;
    END CATCH
END;
go
```

#### 8. Handles the payment process for a product in an order.

```sql
CREATE PROCEDURE Add_Payment
    @OrderID INT,
    @ProductID INT,
    @Amount MONEY
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Order_Details WHERE order_id = @OrderID AND product_id = @ProductID)
        BEGIN
            THROW 50000, 'Order or Product with the given ID does not exist.', 1;
        END

        DECLARE @Defered BIT;
        DECLARE @PaymentAmount MONEY;
        DECLARE @Price MONEY;

        SET @Defered = (SELECT deferred FROM Order_Details WHERE order_id = @OrderID AND product_id = @ProductID);
        SET @PaymentAmount = (SELECT payment_amount FROM Payments WHERE order_id = @OrderID AND product_id = @ProductID);
        SET @Price = (SELECT price FROM Order_Details WHERE order_id = @OrderID AND product_id = @ProductID);

        IF @PaymentAmount + @Amount > @Price
        BEGIN
            THROW 50001, 'Defered payment amount cannot exceed the price of the product.', 1;
        END

        IF @Defered = 0 AND @PaymentAmount + @Amount != @Price
        BEGIN
            THROW 50002, 'Full payment must be made for non-defered products.', 1;
        END

        INSERT INTO Payments (order_id, product_id, payment_amount, payment_date)
        VALUES (@OrderID, @ProductID, @Amount, GETDATE());

        DECLARE @PaymentID INT;
        SET @PaymentID = SCOPE_IDENTITY();

        COMMIT TRANSACTION;

    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        THROW;
    END CATCH
END;
go
```

#### 9. Marks a student as present at a meeting.

```sql
CREATE PROCEDURE Add_Meeting_Presence
    @StudentID INT,
    @MeetingID INT,
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Students WHERE student_id = @StudentID)
        BEGIN
            THROW 50000, 'Student with the given ID does not exist.', 1;
        END

        IF NOT EXISTS (SELECT 1 FROM Meetings WHERE meeting_id = @MeetingID)
        BEGIN
            THROW 50001, 'Meeting with the given ID does not exist.', 1;
        END
        DECLARE @UserID INT;
        SELECT @UserID = users_id FROM Students WHERE student_id = @StudentID;
        IF NOT EXISTS (SELECT 1 FROM GetUserMeetings(@UserID) WHERE meeting_id = @MeetingID)
        BEGIN
            THROW 50002, 'Student is not allowed to attend the meeting.', 1;
        END

        INSERT INTO Meetings_Presence (student_id, meeting_id);

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END;
GO
```

#### 10. Removes a student's presence from a meeting.

```sql
CREATE PROCEDURE Remove_Meeting_Presence
    @StudentID INT,
    @MeetingID INT,
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Students WHERE student_id = @StudentID)
        BEGIN
            THROW 50000, 'Student with the given ID does not exist.', 1;
        END

        IF NOT EXISTS (SELECT 1 FROM Meetings WHERE meeting_id = @MeetingID)
        BEGIN
            THROW 50001, 'Meeting with the given ID does not exist.', 1;
        END

        IF NOT EXISTS (SELECT 1 FROM Meetings_Presence WHERE student_id = @StudentID AND meeting_id = @MeetingID)
        BEGIN
            THROW 50002, 'Student is not present at the meeting.', 1;
        END

        DELETE FROM Meetings_Presence
        WHERE student_id = @StudentID AND meeting_id = @MeetingID;

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END;
```

#### 11. Adds an internship to a study and links it to the given study period.

```sql
CREATE PROCEDURE Add_Internship_To_Study
    @StudyID INT,
    @InternshipID INT,
    @StartDate DATETIME,
    @EndDate DATETIME
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        SET NOCOUNT ON;

        IF NOT EXISTS (SELECT 1 FROM Studies WHERE product_id = @StudyID)
        BEGIN
            THROW 50000, 'Study with the given ID does not exist.', 1;
        END

        IF NOT EXISTS (SELECT 1 FROM Internship WHERE internship_id = @InternshipID)
        BEGIN
            THROW 50001, 'Internship with the given ID does not exist.', 1;
        END

        IF EXISTS (SELECT 1 FROM Internship WHERE study_id = @StudyID AND internship_id = @InternshipID)
        BEGIN
            THROW 50002, 'Internship is already assigned to the Study.', 1;
        END

        INSERT INTO Internship (internship_id, study_id, start_date, end_date)
        VALUES (@InternshipID, @StudyID, @StartDate, @EndDate);

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END;
```
