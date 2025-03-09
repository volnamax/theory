Функция partition() позволяет разделить коллекцию на две части по заданному условию (предикату). Она возвращает Pair из двух списков:

    Первый список содержит элементы, удовлетворяющие условию.
    Второй список содержит все остальные элементы.

📌 Синтаксис:

val (matching, notMatching) = list.partition { condition }

    list — исходная коллекция.
    condition — предикат (условие), по которому разделяются элементы.
    matching — элементы, которые соответствуют условию.
    notMatching — элементы, которые не соответствуют условию.

🛠 Примеры использования:
1️⃣ Разделение списка чисел
```kotlin 
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

// Разделяем на четные и нечетные числа
val (evenNumbers, oddNumbers) = numbers.partition { it % 2 == 0 }

println(evenNumbers)  // [2, 4, 6, 8, 10]
println(oddNumbers)   // [1, 3, 5, 7, 9]
```

✅ Четные числа попадают в первый список, а нечетные – во второй.
2️⃣ Фильтрация положительных и отрицательных чисел
```kotlin 
val nums = listOf(-5, -2, 0, 3, 8, -1, 6)

// Разделяем на положительные и отрицательные
val (positive, negative) = nums.partition { it >= 0 }

println(positive)  // [0, 3, 8, 6]
println(negative)  // [-5, -2, -1]
```

✅ Все положительные числа (и 0) в первом списке, отрицательные – во втором.
3️⃣ Разделение студентов на сдавших и не сдавших экзамен
```kotlin 
data class Student(val name: String, val score: Int)

val students = listOf(
    Student("Alice", 85),
    Student("Bob", 40),
    Student("Charlie", 75),
    Student("David", 30)
)

// Разделяем студентов на сдавших и не сдавших (порог 50 баллов)
val (passed, failed) = students.partition { it.score >= 50 }

println(passed)  // [Student(name=Alice, score=85), Student(name=Charlie, score=75)]
println(failed)  // [Student(name=Bob, score=40), Student(name=David, score=30)]

```

✅ Те, у кого score >= 50, попадают в passed, остальные – в failed.
4️⃣ Разделение строк по длине
```kotlin
val words = listOf("apple", "banana", "cat", "dog", "elephant")

// Разделяем на короткие (≤3 символа) и длинные слова
val (shortWords, longWords) = words.partition { it.length <= 3 }

println(shortWords)  // [cat, dog]
println(longWords)   // [apple, banana, elephant]
```

✅ Короткие слова (до 3 букв) в первом списке, остальные – во втором.
📌 Когда использовать partition()?

    Когда нужно разделить коллекцию на две группы по условию.
    Когда обе части списка должны быть доступны одновременно.
    Когда удобнее работать с двумя списками, а не с одним отфильтрованным.

## **📌 `partition()` vs `filter()`**

|Метод|Возвращает|Использование|
|---|---|---|
|`filter {}`|Один список с элементами, удовлетворяющими условию|`val result = list.filter { it > 0 }`|
|`partition {}`|Два списка: один с удовлетворяющими, другой с оставшимися элементами|`val (pos, neg) = list.partition { it > 0 }`|

✅ **Если нужен только один список — `filter()`. Если нужны оба — `partition()`.**

[[kotlin]]