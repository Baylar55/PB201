# Entity Framework - Console Application

### Library class
- Id 
- Name

### Book class 
- Id
- Name
- PublishYear (_tipi DateTime olmalıdır_)
- Genre (_enum olacaq_)

### Author class
- Id
- Name
- Nationality (_enum olacaq_) 


> [!NOTE]
>**Library ilə book arasında one-to-many relation olacaq.Bir library-də birdən çox kitab ola bilər. Book ilə author arasında many-to-many relation olacaq. Relationlar düzgün qurulmalıdır.**


**Tapşırıqlar aşağıdakı kimidir :**
```
0. Exit
1. Show All Libraries
2. Show All Books
3. Show All Authors
4. Show Books by Library
5. Show Books by Author
6. Show Authors by Books
7. Show All Books with Authors
8. Show All Libraries with Books
9. Add Library
10. Add Book
11. Add Author
12. Assign Author to Book
13. Find Book by Title
14. Find Author by Name
15. Find Books By Genre
```