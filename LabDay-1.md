# Lab Day 1 : Variables, Data types, Operators, Conditions, For Loop



### Task 1 :
**2 dəyişəniniz olacaq və bunlara ədəd mənimsədirsiz. Həmin ədədlər də daxil olmaqla bu iki ədəd arasında neçə cüt, neçə tək ədəd olduğunu ekrana çıxartsın.**

```csharp
public class Program
{
    public static void Main()
    {
        int start = 1;      // start = başlangıc ədəd
        int end = 10;       // end = bitiş ədəd

        int oddNumberCount = 0;      // oddNumberCount = tək Ədəd Sayı
        int evenNumberCount = 0;     // evenNumberCount = cüt Ədəd Sayı

        for (int i = start; i <= end; i++)
        {
            if (i % 2 == 0)
            {
                evenNumberCount++;      // və ya --> evenNumberCount += 1;   və ya --> evenNumberCount = evenNumberCount + 1;
            }
            else
            {
                oddNumberCount++;       // və ya --> oddNumberCount += 1;   və ya --> oddNumberCount = oddNumberCount + 1;
            }
        }

        Console.WriteLine("Odd Number Count : " + oddNumberCount);      // və ya --> Console.WriteLine($"Odd Number Count : {oddNumberCount}");        
        Console.WriteLine("Even Number Count : " + evenNumberCount);    // və ya --> Console.WriteLine($"Even Number Count : {evenNumberCount}");
    }
}
```


### Task 2 :
**2 dəyişəniniz olacaq və bunlara ədəd mənimsədirsiz. Həmin ədədlər də daxil olmaqla bu iki ədəd arasındakı bütün ədədlərin cəmini ekrana çıxartsın.** 

```csharp
public class Program
{
    public static void Main()
    {
        int start = 1;      // start = başlangıc ədəd
        int end = 5;       // end = bitiş ədəd

        int sum = 0;        // sum = cəm

        for (int i = start; i <= end; i++)
        {
            sum = sum + i;      // və ya --> sum += i;

            // izah: 
            // 1-ci dövr: sum = 0 + 1 = 1
            // 2-ci dövr: sum = 1 + 2 = 3
            // 3-cü dövr: sum = 3 + 3 = 6
            // 4-cü dövr: sum = 6 + 4 = 10
            // 5-ci dövr: sum = 10 + 5 = 15
        }

        Console.WriteLine(sum); 
    }
}
```

### Task 3 :
**Ədədin faktorialının tapılması. Faktorial, müəyyən bir ədədin 1-dən başlayaraq özünə qədər olan bütün ədədlərin hasilini tapmaq üçün istifadə olunur. Məsələn :     5! = 1 x 2 x 3 x 4 x 5 = 20**

```csharp
public class Program
{
    public static void Main()
    {
        int number = 5;         // faktorialını tapmaq istədiyimiz ədəd

        int factorial = 1;      // faktorialın hesablanması üçün 1-dən başlayırıq

        for (int i = 1; i <= number; i++)
        {
            factorial = factorial * i;      // və ya -->  factorial *= i;

            //izah:
            // 1. dövr: 1 * 1 = 1
            // 2. dövr: 1 * 2 = 2
            // 3. dövr: 2 * 3 = 6
            // 4. dövr: 6 * 4 = 24
            // 5. dövr: 24 * 5 = 120
        }

        Console.WriteLine(factorial);       
    }
}
```

### Task 4 :
**Vurma cədvəli. Bir ədədiniz olacaq. Bu ədəd üçün vurma cədvəlini aşağıdakı kimi konsola çıxardın. Məsələn 7 ədədini aşağıdakı kimi göstərsin:**
``` 
 7 x 1 = 7
 7 x 2 = 14
 7 x 3 = 21
 7 x 4 = 28
 7 x 5 = 35
 7 x 6 = 42
 7 x 7 = 49
 7 x 8 = 56
 7 x 9 = 63
 7 x 10 = 70 
 ```

```cs
public class Program
{
    public static void Main()
    {
        int number = 7;         // vurma cədvəlini yazdırmaq istədiyiniz ədədi dəyişkənə mənimsədin

        for (int i = 1; i <= 10; i++)
        {
            Console.WriteLine($"{number} x {i} = {number * i}");    

            //İzah:
            //{number} vurma cədvəlini yazdırmaq istədiyiniz ədədi göstərir, {i} isə vurma cədvəlinin hər bir elementini göstərir. {number * i} isə hər bir elementin hasilini göstərir.
        }
    }
}
```

### Task 5 :
**Ədədin qüvvətinin tapılması üçün alqoritm yazın. 2 dəyişəniniz olacaq. 1 - ci dəyişən qüvvətə yüksəltmək istədiyiniz ədəd, ikinci dəyişən qüvvət olacaq. Ədəd qüvvətə yüksəldikdən sonra ekrana çap olunacaq. Məsələn:  2 ^ 4 = 16 (2 üstü 4 bərabərdir 16)**

```csharp
public class Program
{
    public static void Main()
    {
        int number = 2;     // qüvvətə yüksəltmək istədiyiniz ədəd
        int power = 4;      // qüvvət

        int result = 1;     // nəticənin qeyd olunacağı dəyişən

        for (int i = 0; i < power; i++)
        {
            result = result * number;       //və ya --> result *= number;

            //izah:
            // 1. dövr: result = 1 * 2 = 2
            // 2. dövr: result = 2 * 2 = 4
            // 3. dövr: result = 4 * 2 = 8
            // 4. dövr: result = 8 * 2 = 16
        }

        Console.WriteLine(result);
    }
}
```

### Task 6 :
**Aşağıdakı kimi görüntü yaratmaq üçün alqoritm yazın. Hər sətirdə rəqəmlərin sayı bir-bir artmalı və fərqli rəqəmlər yazılmalıdır.**

```
1
2 3
4 5 6
7 8 9 10
```

```csharp

public class Program
{
    public static void Main()
    {
        int n = 4;      // n çap etmək istədiyimiz sətirlərin sayıdır

        int number = 1;     // number dəyişəni ədədləri yazdırmaq və artırmaq üçün istifadə olunacaq

        for (int i = 1; i <= n; i++)    // i 1-dən n-ə qədər artırılacaq, n sətirlərin sayıdır. 
        {
            for (int j = 1; j <= i; j++)    // j 1-dən i-ə qədər artırılacaq, i sətrin elementlərinin sayıdır
            {
                Console.Write(number + " ");    // number dəyişənini yazdırırıq, aralarına boşluq qoyuruq
                number++;       // number dəyişənini artırırıq

                //izah:
                // 1-ci sətrin 1-ci elementi yazdırılır, number 1-dən 2-yə artırılır
                // 2-ci sətrin 1-ci elementi yazdırılır, number 2-dən 3-ə artırılır
                // 2-ci sətrin 2-ci elementi yazdırılır, number 3-dən 4-ə artırılır
                // 3-cü sətrin 1-ci elementi yazdırılır, number 4-dən 5-ə artırılır
                // 3-cü sətrin 2-ci elementi yazdırılır, number 5-dən 6-ya artırılır
                // 3-cü sətrin 3-cü elementi yazdırılır, number 6-dan 7-ye artırılır
                // 4-cü sətrin 1-ci elementi yazdırılır, number 7-dən 8-ə artırılır
                // 4-cü sətrin 2-ci elementi yazdırılır, number 8-dən 9-a artırılır
                // 4-cü sətrin 3-cü elementi yazdırılır, number 9-dan 10-a artırılır
            }
            Console.WriteLine();    // hər sətrin sonunda yeni sətirə keçir
        }
    }
}
```