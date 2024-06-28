# Lab SQL : Joins, Relations,Views, Aggregate Functions



### DemoApp adında bir database yaradılmalıdır.
### People adında bir table yaradılmalıdır:
- Id
- Name
- Surname
- PhoneNumber
- Email
- Age
- Gender
- HasCitizenship
### Countries table yaradılmalıdır:
- Id
- Name
- Area (decimal)
### Cities table yaradılmalıdır:
- Id
- Name
- Area (decimal)

> [!WARNING]
>**Columnlar yaradılarkən constraintlər düzgün istifadə olunmaldır. Table-lar arasında relationlar düzgün qurulmalıdır.** 

> [!NOTE]
>**Bir ölkənin birdən çox şəhəri ola bilər. Bir şəhərdə birdən çox insan ola bilər. Relationlar one to many olacaq. People table-na yeni data əlavə olunanda PhoneNumber column-a əgər data daxil edilmirsə default olaraq bir dəyər yazılmalıdır. Gender column-u dəyər olaraq sadəcə M və ya F ala bilər.**

### Sub Tasks
**1. Joinlərdən istifadə edərək hər üç table birləşdirilməlidir, və nəticədə hər bir personun hansı ölkə və hansı şəhərə aid olduğu viewda əks olunmalıdır. Yazılmış bu join query bir 'View'-a assign olunmalıdır və yenidən birbaşa ordan oxunmalıdır.**

**2. Countries table-ı 'Area' -nın artma sırası ilə sıralanmalıdır.**

**3. Cities table-ı  'Name'-ə görə əks alphabetic sıra ilə sıralanmalıdır.**

**4. Aggregate functionların birindən istifadə edərək 'Area'-sı 20.000-dən çox olan ölkələrin sayı göstərilməlidir**

**5. Aggregate functionlardan istifadə edərək 'Name'-i İ hərfi ilə başlayan ölkələrin arasından ərazi ən böyük olanın 'Area'-sı göstərilməlidir**

**6. Unionlardan istifadə edərək ümumiyyətlə hansı ölkə və şəhərlərin olduğu göstərilməlidir.**

**7. GroupBy istifadə edərək hər şəhərdə neçə person olduğu göstərilməlidir.**

**8.  Növbəti mərhələdə yalnız Personu 5-dən çox olan şəhərlər göstərilməlidir.**

### Solutions : 

```sql
CREATE DATABASE DemoApp
USE DemoApp

CREATE TABLE Countries(
Id int PRIMARY KEY IDENTITY,
Name nvarchar(50) NOT NULL UNIQUE,
Area decimal CHECK(AREA>0)
)

CREATE TABLE Cities(
Id int PRIMARY KEY IDENTITY,
Name nvarchar(50) NOT NULL UNIQUE,
Area decimal CHECK(AREA>0),
CountryId int FOREIGN KEY REFERENCES Countries(Id)
)

CREATE TABLE People(
Id int PRIMARY KEY IDENTITY,
Name nvarchar(50) NOT NULL,
Surname nvarchar(50) NOT NULL,
PhoneNumber nvarchar(50) DEFAULT('+994500000000'),
Email nvarchar(50) NOT NULL UNIQUE,
Age int CHECK(Age>0),
Gender CHAR(1) NULL CHECK (Gender='M' OR Gender='F'),
HasCitizenship bit,
CityId int FOREIGN KEY REFERENCES Cities(Id)
)
INSERT INTO Countries
VALUES('Azerbaijan',48000),
('Germany',60000),
('USA',90000),
('Turkey',80000),
('Ireland',58000),
('Israil',70000)



INSERT INTO Cities
VALUES('Baku',18000,1),
('Gabala',5000,1),
('Munich',25000,2),
('Koln',10000,2),
('Hatay',11000,4),
('Yozgat',15000,4),
('Ceyhan',12000,4)


INSERT INTO People
VALUES('Tahir','Veliyev','+994515350330','tahir@gmail.com',19,'M',1,1),
('Arif','Ahmadov','+994515355462','arif@gmail.com',21,'M',1,1),
('Senuber','Huseynli','+994559098032','senuber@gmail.com',18,'F',1,2),
('Thomas','Muller','+2518030456','muller@gmail.com',32,'M',1,3),
('Leon','Goretzka','+2518030456','goretzka@gmail.com',26,'M',1,4),
('Cihangir','Ceyhan','+9034499232','ceyhan@gamil.com',29,'M',1,7),
('Kerem','Akturkoglu','+9035499432','kerem@agmail.com',23,'M',1,5),
('Ecem','Sena','+9035499456','ecem@gmail.com',25,'F',1,6)
```


### Task 1.
```sql
SELECT  p.Name 'PersonName',p.Surname 'PersonSurname', ct.Name 'City', c.Name 'Country' FROM Countries AS c
INNER JOIN Cities AS ct
ON c.Id = ct.CountryId
INNER JOIN People AS p
ON p.CityId=ct.Id


CREATE VIEW PersonCityAndCountry AS
SELECT  p.Name 'PersonName',p.Surname 'PersonSurname', ct.Name 'City', c.Name 'Country' FROM Countries AS c
INNER JOIN Cities AS ct
ON c.Id = ct.CountryId
INNER JOIN People AS p
ON p.CityId=ct.Id

SELECT * FROM PersonCityAndCountry
```

### Task 2.
```sql
SELECT * FROM Countries 
ORDER BY Area
```

### Task 3.
```sql 
SELECT * FROM Cities 
ORDER BY Name DESC
```

### Task 4.
```sql
SELECT COUNT(Area) FROM Countries
WHERE Area>20000
```

### Task 5.
```sql
SELECT MAX(Area) AS Area FROM Countries
WHERE Name LIKE 'I%'
```

### Task 6.
```sql
SELECT Name,Area FROM Countries
UNION
SELECT Name,Area FROM Cities
```

### Task 7.
```sql
SELECT ct.Name 'CityName',COUNT(CityId) AS 'PersonCount' FROM Cities AS ct
INNER JOIN People AS p
ON ct.Id=p.CityId
GROUP BY ct.Name
```

### Task 8.
```sql
SELECT ct.Name 'CityName',COUNT(CityId) AS 'PersonCount' FROM Cities AS ct
INNER JOIN People AS p
ON ct.Id=p.CityId
GROUP BY ct.Name
HAVING COUNT(CityId)>5
```