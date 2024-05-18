# Lab Day 2 : Switch case, While, Do-While, Array, Methods

### Task 1 :
**İçərisinə bir neçə integer alan və cüt ədədlərinin cəmi ilə tək ədədlərinin cəmi arasındakı fərqi hesablayan metod yazın.**



```cs
public class Program
{
    public static void Main()
    {
        Console.WriteLine(DifferenceBetweenOddAndEven(1, 2, 34, 5, 80, 84, 23));
    }

    public static int DifferenceBetweenOddAndEven(params int[] numbers)
    {
        int oddSum = 0;     // tək ədədlərin cəmi
        int evenSum = 0;    // cüt ədədlərin cəmi

        foreach (var number in numbers)
        {
            if (number % 2 == 0)
            {
                evenSum += number;
            }
            else
            {
                oddSum += number;
            }
        }

        return oddSum - evenSum;
    }
}
```



### Task 2 :
**ReverseString() metodunuz olacaq. Bu metod parametr olaraq string qəbul edəcək və geriyə string qaytaracaq. Metod, daxil edilən stringi tərsinə çevirib geriyə qaytaracaq. Məsələn :**
- **Input : word = "hello"**
- **Output : "olleh"**


```cs
public class Program
{
    public static void Main()
    {
        Console.WriteLine(ReverseString("hello"));
        Console.WriteLine(ReverseString2("hello"));
        Console.WriteLine(ReverseString3("hello"));
    }

    public static string ReverseString(string word)
    {
        string reversed = "";
        for (int i = word.Length - 1; i >= 0; i--)
        {
            reversed += word[i];
        }
        return reversed;
    }

    public static string ReverseString2(string word)
    {
        string reversed = "";
        for (int i = 0; i < word.Length; i++)
        {
            reversed = word[i] + reversed;  // hər bir hərfi əvvələ əlavə edir
        }
        return reversed;
    }

    public static string ReverseString3(string word)
    {
        string reversed = "";
        for (int i = 0; i < word.Length; i++)
        {
            reversed = reversed + word[word.Length-i-1]; 
        }
        return reversed;
    }
}
```


### Task 3 :
**İstifadəçidən adını daxil etməyi istəyən kod yazın. İstifadəçinin daxil etdiyi ad uzunluğu 8-ə bərabər və ya 8-dən uzun deyilsə istifadəçinin yenidən adını soruşsun. Əgər adın uzunluğu 8-ə bərabər və ya 8-dən uzundursa ekranda adı çap edin.**


```cs
public class Program
{
    public static void Main()
    {
        string name="";
        while (name.Length <= 8)
        {
            Console.WriteLine("Enter your name: ");
            name = Console.ReadLine();
        }
        Console.WriteLine(name);
    }
}
```


### Task 4 :
**2 dənə CalculateArea() metodunuz olacaq. Birinci method dairənin sahəsini, ikinci metod isə düzbucaqlının sahəsini hesablayacaq.**

```cs
public class Program
{
    public static void Main()
    {
        CalculateArea(5);
        CalculateArea(5, 10);
    }

    public static int CalculateArea(int radius)
    {
        return 3 * radius * radius;
    }

    public static int CalculateArea(int width, int height)
    {
        return width * height;
    }
}
```


### Task 5 : 
**Verilmiş ədədin ayrı-ayrı rəqəmlərinin cəmini hesablayan metod yazım. Məsələn :**
- **Input : number = 1234**
- **Output : 10**

```cs
public class Program
{
    public static void Main()
    {
        Console.WriteLine(SumOfDigits(1234));
    }

    public static int SumOfDigits(int number)
    {
        int sum = 0;
        while (number > 0)
        {
            sum += number % 10;
            number /= 10;
        }
        return sum;
    }
}
```


### Task 6 :
**Verilmiş sözün palindrom olub-olmadığını yoxlayan metod yazın. Yəni əgər söz həm başdan sona, həm də sondan başa doğru oxunanda bir-birinə bərabərdirsə bu söz palindromdur. Məsələn :**
- **Input : word = "madam"**
- **Output : true**
#
- **Input : word = "kabab"**
- **Output : false**


```cs
public class Program
{
    public static void Main()
    {
        Console.WriteLine(IsPalindrome("madam")); 
        Console.WriteLine(IsPalindrome("hello")); 
        Console.WriteLine(IsPalindrome2("madam"));
        Console.WriteLine(IsPalindrome2("hello"));
        Console.WriteLine(IsPalindrome3("madam"));
        Console.WriteLine(IsPalindrome3("hello")); 
    }

    public static bool IsPalindrome(string s)
    {
        int left = 0;   
        int right = s.Length - 1;

        while (left < right)
        {
            if (s[left] != s[right])
            {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }

    public static bool IsPalindrome2(string word)
    {
        string reverse = "";
        for (int i = word.Length - 1; i >= 0; i--)
        {
            reverse += word[i];
        }

        if(word == reverse)
        {
            return true;
        }
        return false;
    }

    public static bool IsPalindrome3(string word)
    {
        for (int i = 0; i < word.Length/2; i++)
        {
            if (word[i] != word[word.Length - 1 - i])
            {
                return false;
            }
        }
        return true;
    }   

}
```

### Task 7 :
**ConcatArray() metodunuz olacaq. Bu metod parametr olaraq int tipində array qəbul edəcək, geriyə də yenə int tipində array qaytaracaq. Geriyə qaytarılan array parametr kimi verdiyimiz arrayın özü ilə birləşməsi olacaq. Məsələn :**
- **Input : nums = [1,2,3]**
- **Output : [1,2,3,1,2,3]**


```cs
public class Program
{
    public static void Main()
    {
        int[] nums = { 1, 2, 3 };

        int[] result = ConcatArray(nums);
        
        foreach (var item in result)
        {
            Console.WriteLine(item);
        }
    }
    
    public static int[] ConcatArray(int[] nums)
    {
        int[] result = new int[nums.Length * 2];
        for (int i = 0; i < nums.Length; i++)
        {
            result[i] = nums[i];
            result[i + nums.Length] = nums[i];
        }
        return result;
    }
}
```


### Task 8 :
**Student anonim obyektiniz olacaq. Student-in Id, Name, Age, Grade, Class property-ləri olacaq. 4 metodunuz olacaq.**

- **Birinci metod balı 60-dan çox olan və 5-ci sinifdən aşağı siniflərdə oxuyan tələbələrin bütün property-lərini ekrana çıxartsın.**

```cs
public static void DisplayQualifiedStudents(dynamic[] students)
{
    foreach (var student in students)
    {
        if (student.Class < 5 && student.Grade > 60)
        {
            Console.WriteLine($"Id: {student.Id}, Name: {student.Name}, Age: {student.Age}, Grade: {student.Grade}, Class: {student.Class}");
        }
    }
}
```

- **Ikinci metod sadəcə 7-ci sinifdə oxuyan tələbələrin ortalama balını geriyə qaytarsın.**

```cs
public static double GetAvgScoreOf7thGradeStudents(dynamic[] students)
{
    var totalScore = 0;
    var studentCount = 0;

    foreach (var student in students)
    {
        if (student.Class == 7)
        {
            totalScore += student.Grade;
            studentCount++;
        }
    }

    return totalScore / studentCount;
}
```

- **Üçüncü metod 6-cı sinifdə oxuyan Arif adlı tələbənin balı 70-dən çoxdursa əgər bütün məlumatlarını ekrana çıxartsın.**

```cs
public static void DisplayStudentDetail(dynamic[] students)
{
    foreach (var student in students)
    {
        if (student.Name == "Arif" && student.Class == 6 && student.Grade > 70)
        {
            Console.WriteLine($"Id: {student.Id}, Name: {student.Name}, Age: {student.Age}, Grade: {student.Grade}, Class: {student.Class}");
        }
    }
}
```

- **Dördüncü metod isə balı ən yüksək olan tələbənin adını və balını ekrana çıxardacaq.**

```cs
public static void DisplayStudentWithHighestScore(dynamic[] students)
{
    var highestScore = 0;
    var studentName = "";

    foreach (var student in students)
    {
        if (student.Grade > highestScore)
        {
            highestScore = student.Grade;
            studentName = student.Name;
        }
    }

    Console.WriteLine($"Student with the highest score: {studentName}, Score: {highestScore}");
}
```

### Yuxarıdakı metodların çağırılması:

```cs
public class Program
{
    public static void Main()
    {
        var student1 = new { Id = 1, Name = "Arif", Age = 12, Grade = 75, Class = 6 };
        var student2 = new { Id = 2, Name = "Ali", Age = 13, Grade = 65, Class = 3 };
        var student3 = new { Id = 3, Name = "Veli", Age = 11, Grade = 46, Class = 5 };
        var student4 = new { Id = 4, Name = "Malik", Age = 14, Grade = 78, Class = 6 };
        var student5 = new { Id = 5, Name = "Fatma", Age = 10, Grade = 32, Class = 4 };
        var student6 = new { Id = 6, Name = "Irade", Age = 15, Grade = 91, Class = 9 };
        var student7 = new { Id = 7, Name = "Islam", Age = 12, Grade = 80, Class = 7 };
        var student8 = new { Id = 8, Name = "Edalet", Age = 12, Grade = 40, Class = 7 };

        var students = new[] { student1, student2, student3, student4, student5, student6, student7, student8 };

        DisplayQualifiedStudents(students);

        var avgScore = GetAvgScoreOf7thGradeStudents(students);
        Console.WriteLine($"Average score of 7th grade students: {avgScore}");

        DisplayStudentDetail(students);

        DisplayStudentWithHighestScore(students);
    }
}
```



> [!WARNING]
>**Metodların hər biri dynamic[] students parametri alır. Burdakı dynamic keyword-nə ilişməyin. Vaxtı gələndə keçəciyik. Sadəcə bilin ki burdakı anonim student obyektlərindən ibarət arrayı metoda ötürmək üçün istifadə etməliydik bu case-də. Bildiyiniz metodun içinə int[] array, və ya string[] array göndərmək kimidir. Siz sadəcə metodların içərisində hansı işlər görülüb ona diqqət edin.** 



### Task 9 :
**İçərisinə integer arrayi və bir integer alan metodunuz olacaq. Metod içərisinə aldığınız integer sizin hədəf ədədiniz olacaq. Belə ki əgər array içərisində həmin bu hədəf ədədinizə bərabər və ya böyük neçə dənə ədəd varsa onun sayını geriyə qaytaracaq. Məsələn :**
- **Input : int[] nums = [0,1,2,3,4], int target = 1**
- **Output : 4**
- **Izah : 1 ədədinə bərabər və ya ondan böyük 4 ədəd var**
---
- **Input : int[] nums = [5,1,4,2,2], int target = 6**
- **Output : 0**
- **Izah : 6 ədədinə bərabər və ya ondan böyük ədəd yoxdur**

```cs
public class Program
{
    public static void Main()
    {
        int target = 5;
        int[] numbers = { 1, 9, 3, 19, 5, 6, 7, 8, 9, 10 };
        Console.WriteLine(CountNumbers(target, numbers)); 
    }

    public static int CountNumbers(int target, int[] numbers)
    {
        int count = 0;
        foreach (var number in numbers)
        {
            if (number >= target)
            {
                count++;
            }
        }
        return count;
    }
}
```

### Task 10 :
**İçərisinə integer arrayi alan bir metod yazın. Həmin bu arrayin içində təkrarlanan ədədlər olacaq. Metod  geriyə sadəcə unikal elementlərdən yəni təkrarlanmayan elementlərdən ibarət array qaytarmalıdır. Məsələn :**

- **Input : int[] nums = [ 1, 12, 3, 4, 4, 6, 1, 9, 12]**
- **Output : [1, 12, 3, 4, 6, 9]**


```cs
public class Program
{
    public static void Main()
    {
        var arr = new int[] { 1, 12, 3, 4, 4,6, 1,9,12 };
        var uniqueElements = GetUniqueElements(arr);

        foreach (var element in uniqueElements)
        {
            Console.WriteLine(element);
        }
    }

    public static int[] GetUniqueElements(int[] numbers)
    {
        int n = numbers.Length;
        int uniqueCount = 0;

        for (int i = 0; i < n; i++)
        {
            bool isDuplicate = false;
            for (int j = 0; j < uniqueCount; j++)
            {
                if (numbers[i] == numbers[j])
                {
                    isDuplicate = true;
                    break;
                }
            }
            if (!isDuplicate)
            {
                numbers[uniqueCount] = numbers[i];
                uniqueCount++;
            }
        }

        int[] uniqueElements = new int[uniqueCount];
        for (int i = 0; i < uniqueCount; i++)
        {
            uniqueElements[i] = numbers[i];
        }

        return uniqueElements;
    }
}
```