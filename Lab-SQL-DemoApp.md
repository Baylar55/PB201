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
