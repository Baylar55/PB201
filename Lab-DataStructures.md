# Lab - Data Structures 



### Task 1 :
**Verilmiş stringdə hər bir hərfin neçə dəfə işlədildiyini göstərən metod yazın. Metod string word qəbul edəcək və hər bir hərfin neçə dəfə işlədildiyini ekrana yazdıracaq.**

```csharp
public static void DisplayCharCount(string word)
{
    Dictionary<char, int> dict = new();
    
    foreach (var item in word)
    {
        if (!dict.ContainsKey(item))
            dict.Add(item, 1);
        else
            dict[item]++;
    }

    foreach (var item in dict)
    {
        Console.WriteLine(item.Key + " - " + item.Value);
    }
}
```

### Task 2 :
**Tərkibində ulduzlar istifadə edilmiş sözdəki hər bir ulduzu və həmin ulduzdan əvvəlki hərfi silib geriyə yerdə qalan hərflərdən ibarət string qaytaran metod yazın. Məsələn:**

```
string word = "gl**ob*al";
1-ci addım --> g*ob*al
2-ci addım --> ob*al
3-cü addım --> oal

Metod geriyə   oal qaytarmalıdır.
```

```csharp
public static string RemoveStars(string word)
{
    Stack<char> st = new();

    foreach (var item in word)
    {
        if (item != '*')
            st.Push(item);
        else
            st.Pop();
    }
    return new string(st.Reverse().ToArray());
}
```

### Task 3 :
**Geriyə true,false qaytaran parametr olaraq sadəcə ( , ) , { , } , [ , ]  bu bracketlərdən ibarət string qəbul edən metod yazın. Daxil edilən string yalnız aşağıdakı qaydalar daxilində true qaytarmaldır.**

- Açılan bracket eyni tipdə olan bracket ilə bağlanmalıdır.
- Açılan bracketlər düzgün sıra ilə bağlanmalıdır.
```
 Example 1 : 
    Input : s = "()[]{}"
    Output: true
 Example 2 : 
    Input : s = "()[}{]"
    Output: false
 Example 3 :
    Input : s = "(]"
    Output: false
 Example 4 : 
    Input : s = "([]{})"
    Output: true
```
```csharp
public static bool IsValid(string s)
{
    Stack<char> chars = new();
    char peek;
    foreach (var item in s)
    {
        if (item == '(' || item == '[' || item == '{')
            chars.Push(item);
        
        else if (chars.Count() != 0)
        {
            peek = chars.Peek();
        
            if (peek == '{' && item == '}')
            {
                chars.Pop();
                continue;
            }
            if (peek == '[' && item == ']')
            {
                chars.Pop();
                continue;
            }
            if (peek == '(' && item == ')')
            {
                chars.Pop();
                continue;
            }
            return false;
        }
        else
            return false;

    }
    return chars.Count == 0;
}
```
```csharp
public static bool IsValid(string s)
{
    Dictionary<char, char> bracketsMap = new Dictionary<char, char>{
            {'{',  '}'},
            {'(',  ')'},
            {'[',  ']'}, };
    Stack<char> openBrackets = new Stack<char>();

    foreach (char bracket in s)
    {
        if (bracketsMap.ContainsKey(bracket))
        {
            openBrackets.Push(bracket);
        }
        else
        {
            if (openBrackets.Count == 0)
            {
                return false;
            }
            if (bracketsMap[openBrackets.Pop()] == bracket)
            {
                continue;
            };
            return false;
        }
    }
    return openBrackets.Count == 0;
}
```


