# Generating test data

#### 1. Generating data for table Countries

```sql
INSERT INTO Countries (country_label) VALUES
                                          ('Afghanistan'),
                                          ('Albania'),
                                          ('Algeria'),
                                          ('Andorra'),
                                          ('Angola'),
                                          ('Antigua and Barbuda'),
                                          ('Argentina'),
                                          ('Armenia'),
                                          ('Australia'),
                                          ('Austria'),
                                          ('Azerbaijan'),
                                          ('Bahamas'),
                                          ('Bahrain'),
                                          ('Bangladesh'),
                                          ('Barbados'),
                                          ('Belarus'),
                                          ('Belgium'),
                                          ('Belize'),
                                          ('Benin'),
                                          ('Bhutan'),
                                          ('Bolivia'),
                                          ('Bosnia and Herzegovina'),
                                          ('Botswana'),
                                          ('Brazil'),
                                          ('Brunei'),
                                          ('Bulgaria'),
                                          ('Burkina Faso'),
                                          ('Burundi'),
                                          ('Cabo Verde'),
                                          ('Cambodia'),
                                          ('Cameroon'),
                                          ('Canada'),
                                          ('Central African Republic'),
                                          ('Chad'),
                                          ('Chile'),
                                          ('China'),
                                          ('Colombia'),
                                          ('Comoros'),
                                          ('Congo (Congo-Brazzaville)'),
                                          ('Costa Rica'),
                                          ('Croatia'),
                                          ('Cuba'),
                                          ('Cyprus'),
                                          ('Czechia'),
                                          ('Denmark'),
                                          ('Djibouti'),
                                          ('Dominica'),
                                          ('Dominican Republic'),
                                          ('Ecuador'),
                                          ('Egypt'),
                                          ('El Salvador'),
                                          ('Equatorial Guinea'),
                                          ('Eritrea'),
                                          ('Estonia'),
                                          ('Eswatini (fmr. "Swaziland")'),
                                          ('Ethiopia'),
                                          ('Fiji'),
                                          ('Finland'),
                                          ('France'),
                                          ('Gabon'),
                                          ('Gambia'),
                                          ('Georgia'),
                                          ('Germany'),
                                          ('Ghana'),
                                          ('Greece'),
                                          ('Grenada'),
                                          ('Guatemala'),
                                          ('Guinea'),
                                          ('Guinea-Bissau'),
                                          ('Guyana'),
                                          ('Haiti'),
                                          ('Holy See'),
                                          ('Honduras'),
                                          ('Hungary'),
                                          ('Iceland'),
                                          ('India'),
                                          ('Indonesia'),
                                          ('Iran'),
                                          ('Iraq'),
                                          ('Ireland'),
                                          ('Israel'),
                                          ('Italy'),
                                          ('Jamaica'),
                                          ('Japan'),
                                          ('Jordan'),
                                          ('Kazakhstan'),
                                          ('Kenya'),
                                          ('Kiribati'),
                                          ('Kuwait'),
                                          ('Kyrgyzstan'),
                                          ('Laos'),
                                          ('Latvia'),
                                          ('Lebanon'),
                                          ('Lesotho'),
                                          ('Liberia'),
                                          ('Libya'),
                                          ('Liechtenstein'),
                                          ('Lithuania'),
                                          ('Luxembourg'),
                                          ('Madagascar'),
                                          ('Malawi'),
                                          ('Malaysia'),
                                          ('Maldives'),
                                          ('Mali'),
                                          ('Malta'),
                                          ('Marshall Islands'),
                                          ('Mauritania'),
                                          ('Mauritius'),
                                          ('Mexico'),
                                          ('Micronesia'),
                                          ('Moldova'),
                                          ('Monaco'),
                                          ('Mongolia'),
                                          ('Montenegro'),
                                          ('Morocco'),
                                          ('Mozambique'),
                                          ('Myanmar (formerly Burma)'),
                                          ('Namibia'),
                                          ('Nauru'),
                                          ('Nepal'),
                                          ('Netherlands'),
                                          ('New Zealand'),
                                          ('Nicaragua'),
                                          ('Niger'),
                                          ('Nigeria'),
                                          ('North Korea'),
                                          ('North Macedonia'),
                                          ('Norway'),
                                          ('Oman'),
                                          ('Pakistan'),
                                          ('Palau'),
                                          ('Palestine State'),
                                          ('Panama'),
                                          ('Papua New Guinea'),
                                          ('Paraguay'),
                                          ('Peru'),
                                          ('Philippines'),
                                          ('Poland'),
                                          ('Portugal'),
                                          ('Qatar'),
                                          ('Romania'),
                                          ('Russia'),
                                          ('Rwanda'),
                                          ('Saint Kitts and Nevis'),
                                          ('Saint Lucia'),
                                          ('Saint Vincent and the Grenadines'),
                                          ('Samoa'),
                                          ('San Marino'),
                                          ('Sao Tome and Principe'),
                                          ('Saudi Arabia'),
                                          ('Senegal'),
                                          ('Serbia'),
                                          ('Seychelles'),
                                          ('Sierra Leone'),
                                          ('Singapore'),
                                          ('Slovakia'),
                                          ('Slovenia'),
                                          ('Solomon Islands'),
                                          ('Somalia'),
                                          ('South Africa'),
                                          ('South Korea'),
                                          ('South Sudan'),
                                          ('Spain'),
                                          ('Sri Lanka'),
                                          ('Sudan'),
                                          ('Suriname'),
                                          ('Sweden'),
                                          ('Switzerland'),
                                          ('Syria'),
                                          ('Tajikistan'),
                                          ('Tanzania'),
                                          ('Thailand'),
                                          ('Timor-Leste'),
                                          ('Togo'),
                                          ('Tonga'),
                                          ('Trinidad and Tobago'),
                                          ('Tunisia'),
                                          ('Turkey'),
                                          ('Turkmenistan'),
                                          ('Tuvalu'),
                                          ('Uganda'),
                                          ('Ukraine'),
                                          ('United Arab Emirates'),
                                          ('United Kingdom'),
                                          ('United States of America'),
                                          ('Uruguay'),
                                          ('Uzbekistan'),
                                          ('Vanuatu'),
                                          ('Vatican City'),
                                          ('Venezuela'),
                                          ('Vietnam'),
                                          ('Yemen'),
                                          ('Zambia'),
                                          ('Zimbabwe');
```

#### 2. Generating data for table Languages

```sql
INSERT INTO Languages (language_label) VALUES
                                           ('Abkhaz'),
                                           ('Afar'),
                                           ('Afrikaans'),
                                           ('Akan'),
                                           ('Albanian'),
                                           ('Amharic'),
                                           ('Arabic'),
                                           ('Aragonese'),
                                           ('Armenian'),
                                           ('Assamese'),
                                           ('Avaric'),
                                           ('Avestan'),
                                           ('Aymara'),
                                           ('Azerbaijani'),
                                           ('Bambara'),
                                           ('Bashkir'),
                                           ('Basque'),
                                           ('Belarusian'),
                                           ('Bengali'),
                                           ('Bihari'),
                                           ('Bislama'),
                                           ('Bosnian'),
                                           ('Breton'),
                                           ('Bulgarian'),
                                           ('Burmese'),
                                           ('Catalan'),
                                           ('Chamorro'),
                                           ('Chechen'),
                                           ('Chichewa'),
                                           ('Chinese'),
                                           ('Church Slavic'),
                                           ('Chuvash'),
                                           ('Cornish'),
                                           ('Corsican'),
                                           ('Cree'),
                                           ('Croatian'),
                                           ('Czech'),
                                           ('Danish'),
                                           ('Divehi'),
                                           ('Dutch'),
                                           ('Dzongkha'),
                                           ('English'),
                                           ('Esperanto'),
                                           ('Estonian'),
                                           ('Ewe'),
                                           ('Faroese'),
                                           ('Fijian'),
                                           ('Finnish'),
                                           ('French'),
                                           ('Galician'),
                                           ('Georgian'),
                                           ('German'),
                                           ('Greek'),
                                           ('Guarani'),
                                           ('Gujarati'),
                                           ('Haitian Creole'),
                                           ('Hausa'),
                                           ('Hebrew'),
                                           ('Herero'),
                                           ('Hindi'),
                                           ('Hiri Motu'),
                                           ('Hungarian'),
                                           ('Icelandic'),
                                           ('Ido'),
                                           ('Igbo'),
                                           ('Indonesian'),
                                           ('Interlingua'),
                                           ('Interlingue'),
                                           ('Inuktitut'),
                                           ('Inupiaq'),
                                           ('Irish'),
                                           ('Italian'),
                                           ('Japanese'),
                                           ('Javanese'),
                                           ('Kalaallisut'),
                                           ('Kannada'),
                                           ('Kanuri'),
                                           ('Kashmiri'),
                                           ('Kazakh'),
                                           ('Khmer'),
                                           ('Kikuyu'),
                                           ('Kinyarwanda'),
                                           ('Kirghiz'),
                                           ('Komi'),
                                           ('Kongo'),
                                           ('Korean'),
                                           ('Kurdish'),
                                           ('Kwanyama'),
                                           ('Lao'),
                                           ('Latin'),
                                           ('Latvian'),
                                           ('Limburgish'),
                                           ('Lingala'),
                                           ('Lithuanian'),
                                           ('Luba-Katanga'),
                                           ('Luxembourgish'),
                                           ('Macedonian'),
                                           ('Malagasy'),
                                           ('Malay'),
                                           ('Malayalam'),
                                           ('Maltese'),
                                           ('Manx'),
                                           ('Maori'),
                                           ('Marathi'),
                                           ('Marshallese'),
                                           ('Mongolian'),
                                           ('Nauru'),
                                           ('Navajo'),
                                           ('Ndonga'),
                                           ('Nepali'),
                                           ('North Ndebele'),
                                           ('Northern Sami'),
                                           ('Norwegian'),
                                           ('Norwegian Bokmål'),
                                           ('Norwegian Nynorsk'),
                                           ('Nuosu'),
                                           ('Occitan'),
                                           ('Ojibwa'),
                                           ('Oriya'),
                                           ('Oromo'),
                                           ('Ossetian'),
                                           ('Pali'),
                                           ('Panjabi'),
                                           ('Pashto'),
                                           ('Persian'),
                                           ('Polish'),
                                           ('Portuguese'),
                                           ('Quechua'),
                                           ('Romanian'),
                                           ('Russian'),
                                           ('Samoan'),
                                           ('Sango'),
                                           ('Sanskrit'),
                                           ('Sardinian'),
                                           ('Scottish Gaelic'),
                                           ('Serbian'),
                                           ('Shona'),
                                           ('Sindhi'),
                                           ('Sinhala'),
                                           ('Slovak'),
                                           ('Slovene'),
                                           ('Somali'),
                                           ('South Ndebele'),
                                           ('Southern Sotho'),
                                           ('Spanish'),
                                           ('Sundanese'),
                                           ('Swahili'),
                                           ('Swati'),
                                           ('Swedish'),
                                           ('Tagalog'),
                                           ('Tahitian'),
                                           ('Tajik'),
                                           ('Tamil'),
                                           ('Tatar'),
                                           ('Telugu'),
                                           ('Thai'),
                                           ('Tibetan'),
                                           ('Tigrinya'),
                                           ('Tonga'),
                                           ('Tsonga'),
                                           ('Tswana'),
                                           ('Turkish'),
                                           ('Turkmen'),
                                           ('Twi'),
                                           ('Uighur'),
                                           ('Ukrainian'),
                                           ('Urdu'),
                                           ('Uzbek'),
                                           ('Venda'),
                                           ('Vietnamese'),
                                           ('Volapük'),
                                           ('Walloon'),
                                           ('Welsh'),
                                           ('Western Frisian'),
                                           ('Wolof'),
                                           ('Xhosa'),
                                           ('Yiddish'),
                                           ('Yoruba'),
                                           ('Zhuang'),
                                           ('Zulu');
```

#### 3. Generating data for table Users

```sql
DECLARE @i INT = 1;
DECLARE @firstname NVARCHAR(100);
DECLARE @lastname NVARCHAR(100);
DECLARE @email NVARCHAR(100);
DECLARE @phone NVARCHAR(20);
DECLARE @country NVARCHAR(100) = 'Poland'; -- Ensure this country exists in the Countries table
DECLARE @postal_code NVARCHAR(100);
DECLARE @city NVARCHAR(100);
DECLARE @street NVARCHAR(100);
DECLARE @apartment_no INT;
DECLARE @birth_date DATE;
DECLARE @user_type NVARCHAR(50);

WHILE @i <= 5000 -- Adjust the number of iterations to generate the desired amount of data
    BEGIN
        SET @firstname = CONCAT('FirstName', @i);
        SET @lastname = CONCAT('LastName', @i);
        SET @email = CONCAT('user', @i, '@example.com');
        SET @phone = CONCAT('123-456-', RIGHT('0000' + CAST(@i AS NVARCHAR), 4));
        SET @postal_code = CONCAT('30-', RIGHT('000' + CAST(@i % 1000 AS NVARCHAR), 3));
        SET @city = CONCAT('City', @i % 100);
        SET @street = CONCAT('Street', @i % 50);
        SET @apartment_no = @i % 20 + 1;
        SET @birth_date = DATEADD(DAY, -@i * 10, GETDATE()); -- Random past date
        SET @user_type = CASE
                             WHEN @i % 4 = 0 THEN 'Student'
                             WHEN @i % 4 = 1 THEN 'Translator'
                             WHEN @i % 4 = 2 THEN 'Coordinator'
                             ELSE 'Tutor'
            END;

        EXEC Add_User_With_Type
             @firstname = @firstname,
             @lastname = @lastname,
             @email = @email,
             @phone = @phone,
             @country = @country,
             @postal_code = @postal_code,
             @city = @city,
             @street = @street,
             @apartment_no = @apartment_no,
             @birth_date = @birth_date,
             @user_type = @user_type;

        SET @i = @i + 1;
    END;
```

#### 4. Generating data for table Products

```sql
DECLARE @i INT = 0;
-- Products
DECLARE @product_price MONEY;
DECLARE @product_title VARCHAR(100);
DECLARE @product_description VARCHAR(MAX);
DECLARE @release_date DATETIME;
DECLARE @start_date DATETIME;
-- Product Type
DECLARE @product_type VARCHAR(50); -- Typ produktu: 'Course', 'Study', 'Module', 'Subject', 'Webinar'

-- Other ID
DECLARE @course_id INT = NULL; -- ID kursu, studium itp.
DECLARE @study_id INT = NULL; -- ID studium
DECLARE @tutor_id INT = NULL; -- ID tutora
DECLARE @translator_id INT = NULL; -- ID tłumacza
DECLARE @coordinator_id INT = NULL; -- ID koordynatora
-- Additional
DECLARE @place_limits INT = NULL;  -- Limit miejsc dla kursu, studium itp.
DECLARE @webinar_date DATETIME = NULL; -- Data webinaru (jeśli produkt to webinar)
DECLARE @record_start_date DATETIME = NULL;
DECLARE @record_end_date DATETIME = NULL;
DECLARE @record_url VARCHAR(100) = NULL;
WHILE @i < 500
BEGIN
    SET @coordinator_id = 1251 + FLOOR(RAND() * 25);
    SET @product_price = CAST(FLOOR(RAND() * 100) AS MONEY);
    SET @product_title = 'Product ' + CAST(@i AS VARCHAR);
    SET @product_description = 'Description ' + CAST(@i AS VARCHAR);
    SET @release_date = DATEADD(DAY, FLOOR(RAND() * 60) - 30, GETDATE());
    SET @start_date = DATEADD(DAY, FLOOR(RAND() * 365) - 30, @release_date);
    SET @product_type = CASE @i % 5
        WHEN 0 THEN 'Course'
        WHEN 1 THEN 'Study'
        WHEN 2 THEN 'Module'
        WHEN 3 THEN 'Subject'
        ELSE 'Webinar'
    END;
    SET @place_limits = FLOOR(RAND() * 200) + 100;
    SET @course_id = 411195 + @i - @i % 5;
    SET @study_id = 411195 + @i - @i % 5 + 1;
    SET @webinar_date = DATEADD(DAY, FLOOR(RAND() * 30), @start_date);
    SET @record_start_date = DATEADD(DAY, FLOOR(RAND() * 30), @webinar_date);
    SET @record_end_date = DATEADD(DAY, FLOOR(RAND() * 30), @record_start_date);
    SET @record_url = 'http://www.webinar' + CAST(@i AS VARCHAR) + '.com';
    SET @tutor_id = 1251 + FLOOR(RAND() * 25);
    SET @translator_id = 1251 + FLOOR(RAND() * 25);
    EXEC Add_Product_With_Type
        @product_price,
        @product_title,
        @product_description,
        @release_date,
        @start_date,
        @product_type,
        @course_id,
        @study_id,
        @tutor_id,
        @translator_id,
        @coordinator_id,
        @place_limits,
        @webinar_date,
        @record_start_date,
        @record_end_date,
        @record_url;
    SET @i = @i + 1;
end
```

#### 5. Generating data for table Translator_Languages

```sql
DECLARE @translator_id INT;
DECLARE @language_label VARCHAR(100);

DECLARE TranslatorCursor CURSOR FOR SELECT translator_id FROM Translators;
OPEN TranslatorCursor;

FETCH NEXT FROM TranslatorCursor INTO @translator_id;
WHILE @@FETCH_STATUS = 0
BEGIN
    SET @language_label = (SELECT TOP 1 language_label FROM Languages ORDER BY NEWID());
    EXEC Add_Translator_Language @translator_id, @language_label;
    FETCH NEXT FROM TranslatorCursor INTO @translator_id;
END
```
