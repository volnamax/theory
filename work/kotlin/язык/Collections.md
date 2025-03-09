В **Kotlin** `Collection` — это интерфейс, который наследуется от `Iterable<T>`.
### **Расскажите о Collections в Kotlin**

Kotlin представляет множество стандартных коллекций, таких как списки (List), множества (Set), карты (Map), как изменяемые, так и неизменяемые версии.

Списки List бывают:

```
Неизменяемые (List<T>) → нельзя изменять после создания.
Изменяемые (MutableList<T>) → можно добавлять/удалять элементы.

```

```kotlin
val immutableList = listOf("A", "B", "C")  // Неизменяемый список
// immutableList.add("D") // Ошибка!
```

```kotlin
val mutableList = mutableListOf("X", "Y", "Z")  // Изменяемый список
mutableList.add("W")  // Теперь ["X", "Y", "Z", "W"]
```

📌 Операции с List

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
```

```kotlin
println(numbers.first())  // 1
println(numbers.last())   // 5
println(numbers.get(2))   // 3
println(numbers.contains(3))  // true
println(numbers.indexOf(4))  // 3
```

🔹 Set (Множество)

В Set нет дубликатов, элементы хранятся в произвольном порядке.

```kotlin
val immutableSet = setOf(1, 2, 3, 3, 4)  // [1, 2, 3, 4]
val mutableSet = mutableSetOf(5, 6, 7)
mutableSet.add(7)  // Не добавит, так как 7 уже есть
```

📌 Операции с Set

```kotlin
val set = setOf("apple", "banana", "cherry")
```

```kotlin
println(set.contains("banana")) // true
println(set.size) // 3
println(set.minus("banana")) // [apple, cherry] (не изменяет оригинальный set)
```

🔹 Map (Карта)

```kotlin
Map хранит пары ключ → значение.
```

```kotlin
val immutableMap = mapOf("name" to "Alice", "age" to 25)
val mutableMap = mutableMapOf("apple" to 3, "banana" to 5)
```

```kotlin
mutableMap["banana"] = 6  // Изменяем значение
mutableMap["cherry"] = 10  // Добавляем новый элемент
```

📌 Операции с Map

```kotlin
val map = mapOf("A" to 1, "B" to 2, "C" to 3)
```

```kotlin
println(map.keys)  // [A, B, C]
println(map.values)  // [1, 2, 3]
println(map["A"])  // 1
println(map.containsKey("B"))  // true
println(map.getOrDefault("D", 0))  // 0 (если "D" нет в Map)
```

🔹 Функции для работы с коллекциями

| Функция                          | Описание                                               | Пример                                                                       |
| -------------------------------- | ------------------------------------------------------ | ---------------------------------------------------------------------------- |
| **filter { }**                   | Фильтрует элементы по условию                          | `list.filter { it > 2 } // [3,4,5]`                                          |
| **map { }**                      | Преобразует элементы                                   | `list.map { it * 2 } // [2, 4, 6]`                                           |
| **forEach { }**                  | Итерация по элементам                                  | `list.forEach { println(it) }`                                               |
| **sorted()**                     | Сортировка по возрастанию                              | `list.sorted() // [1,2,3,4,5]`                                               |
| **sortedDescending()**           | Сортировка по убыванию                                 | `list.sortedDescending()`                                                    |
| **groupBy { }**                  | Группировка по признаку                                | `listOf("a", "abc", "ab").groupBy { it.length } // {1=[a], 2=[ab], 3=[abc]}` |
| **reduce { acc, it -> }**        | Свертка элементов (аккумуляция)                        | `listOf(1,2,3).reduce { acc, it -> acc + it } // 6`                          |
| **fold(initial) { acc, it -> }** | Аналог `reduce`, но с начальным значением              | `list.fold(10) { acc, it -> acc + it } // 16`                                |
| **find { }**                     | Находит первый элемент по условию                      | `list.find { it > 2 } // 3`                                                  |
| **firstOrNull { }**              | Безопасный `first()`                                   | `list.firstOrNull { it > 10 } // null`                                       |
| **any { }**                      | Есть ли хотя бы один элемент, удовлетворяющий условию? | `list.any { it > 3 } // true`                                                |
| **all { }**                      | Все ли элементы удовлетворяют условию?                 | `list.all { it > 0 } // true`                                                |
| **none { }**                     | Нет ли элементов, удовлетворяющих условию?             | `list.none { it < 0 } // true`                                               |
| **distinct()**                   | Убирает дубликаты                                      | `listOf(1, 2, 2, 3).distinct() // [1,2,3]`                                   |
| **flatMap { }**                  | Разворачивает вложенные списки                         | `listOf(listOf(1,2), listOf(3,4)).flatMap { it } // [1,2,3,4]`               |
| **chunked(n)**                   | Разделяет список на подсписки по `n` элементов         | `listOf(1,2,3,4,5).chunked(2) // [[1,2], [3,4], [5]]`                        |
| **zip(otherList)**               | Объединяет два списка в пары                           | `listOf(1,2,3).zip(listOf("a", "b", "c")) // [(1, a), (2, b), (3, c)]`       |

Примеры:

```kotlin
val list = listOf(1, 2, 3, 4, 5)
```

```kotlin
println(list.filter { it % 2 == 0 })  // [2, 4]
println(list.map { it * 2 })  // [2, 4, 6, 8, 10]
println(list.sortedDescending())  // [5, 4, 3, 2, 1]
```

```
List → упорядоченные элементы, допускает дубликаты.
Set → уникальные элементы, без дубликатов.
Map → хранит ключ → значение.
Неизменяемые коллекции (listOf, setOf, mapOf) → нельзя менять.
Изменяемые коллекции (mutableListOf, mutableSetOf, mutableMapOf) → можно менять.

```

[[kotlin]]