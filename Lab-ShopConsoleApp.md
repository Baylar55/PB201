### Shop - Console Application

- Shop class:
    - Id
    - Name
    - MinSalary
    - Budget
    - List < Employee >
    - List < Product > 

- Employee
    - Id
    - Name
    - Surname
    - Salary
    - RoleType(Admin/Worker)
    - Username (unique olmalıdır)
    - Password

- Product
    - Id
    - Name
    - ProductType(Electronics,Furniture,Toys)
    - Count
    - PurchasePrice
    - SalePrice

**Application başlayanda ilkin olaraq default bir Shop class-ı yaradılır ve içinə default bir Admin rolunda Employee elave olunur.Application açıldıqda login prosesi başlanır.İstifadəçidən username və parol istənilir əgər, mağaza işçilərinin arasında bu username və parolda işçi varsa, daxil olur. Əks halda daxil ola bilmir. Daxil olduqdan sonra , əgər istifadəçi admin rolundadırsa aşağıdakı seçimlər çıxır.**

1. Admin panel
2. Satış et
3. Məlumatları yenilə

**Əgər istifadəçi staff rolundadırsa aşağıdakı seçimlər çıxır.**
1. Satış et
2. Məlumatlarımı yenilə


**1 - ə click edəndə Admin paneldə aşağıdakı seçimlər olur :**

- *Add Product*
    - Product əlavə edəndə bütün fieldləri alıb sonra əlavə edirsən. Eyni ad və eyni type-da product ola bilməz.Eyni ad fərqli type-da product ola bilər. Product əlavə edərkən büdcədə yetəri qədər pul olub olmadığını yoxlamaq lazımdır.(purchaseprice-a əsasən) 
- *Remove Product*
    - Product adına görə filter edilib həmin productları informasıyasıyla birlikdə ekrana yazdırır. Daha sonra istifadəçidən id alınır və həmin id-yə görə product silinir.
- *Edit Product*
    - Product adına görə filter edilib həmin productları informasıyasıyla birlikdə ekrana yazdırır. Daha sonra istifadəçidən id alınır və həmin id-yə görə product editlənir.
- *Add Employee*
    + Employee əlavə edəndə bütün fieldləri alıb sonra əlavə edirsən. Salary, Shop-un salary şərtinə uyğun olmalıdır.
- *Remove Employee*
    + Employee adına görə filter edilib həmin employeeləri informasıyasıyla birlikdə ekrana yazdırır. Daha sonra istifadəçidən id alınır və həmin id-yə görə employee silinir. 
- *Edit Employee*
    + Employee adına görə filter edilib həmin employeeləri informasıyasıyla birlikdə ekrana yazdırır. Daha sonra istifadəçidən id alınır və həmin id-yə görə employee editlənir. 

**2 - ə click edəndə Satış etdə aşağıdakı kimi olur :**
- Daxil edilən product adına, tipə uyğun product tapılır. Həmin product-ın məlumatları ekranda yazdırılır. 
- Daha sonra istifadəçidən neçə dənə product almaq istədiyi soruşulur və həmin saya uyğun satış edilir.
- Əgər product sayı istifadəçinin daxil etdiyi saydan azdırsa həmin say qədər product istəyib istəmədiyi soruşulur. Əgər istifadəçi yes daxil edirsə satılmalı, no daxil edirsə satılmamalıdır. Edilən satışa görə büdcə dəyişməlidir.

**3 - ə click edəndə Məlumatları yenilədə aşağıdakı kimi olur :**

- Admin sadəcə özünün və staff-ların istənilən məlumatlarını yeniləyə bilər. Digər adminlərə icazə yoxdur. 



> [!WARNING]
>**Ama staff-in məlumatları yenilə hissəsində istifadəçidən username və password istənilir. Əgər username-ə görə password düzgün daxil edilibsə sadəcə ğhəmin istifadəçinin istənilən informasiyasını yeniləyə bilər.** 