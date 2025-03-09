### **Что такое data класс в Kotlin?**

Data-классы в Kotlin предназначены для хранения данных и автоматически создают важные методы, избавляя разработчиков от лишнего кода.

Автоматически создаваемые методы: 
1️⃣ equals() – сравнивает два объекта 
2️⃣ hashCode() – возвращает числовой хэш-код
3️⃣ toString() – строковое представление объекта 
4️⃣ copy() – создание копии с возможностью изменения параметров 
5️⃣ componentN() – позволяет использовать деструктуризацию 

1️⃣ equals()  
Обычно в Kotlin объекты сравниваются по ссылке, но data class переопределяет equals(), чтобы сравнивать по значениям. Пример:
```kotlin 
data class User(val name: String, val age: Int)

fun main() { val user1 = User("Alice", 25) val user2 = User("Alice", 25)
println(user1 == user2) // true, так как значения одинаковые
println(user1 === user2) // false, так как это разные объекты в памяти
```
🔹 В обычном class сравнение user1 == user2 дало бы false, но data class сравнивает по значениям, а не по ссылке. 

📌 2. hashCode() — хэш-код объекта [[HashMap VS HashSet]]

Хэш-код используется для оптимизации работы коллекций (например, HashSet, HashMap). В data class хэш-код генерируется на основе значений свойств, а не адреса в памяти. Пример:
```kotlin 
fun main() { val user1 = User("Alice", 25)
val user2 = User("Alice", 25)
println(user1.hashCode()) // Например, 12345678
println(user2.hashCode()) // Будет такой же, так как значения одинаковые
```
🔹 Важно: если поля объекта изменятся, его hashCode тоже поменяется.
 📌 3. toString() — строковое представление объекта

В data class этот метод автоматически форматирует объект читаемым способом. Пример:
```kotlin
fun main() { val user = User("Alice", 25) println(user)
// Выведет: User(name=Alice, age=25) }
```
🔹 В обычном классе пришлось бы переопределять toString() вручную. 

📌 4. copy() — создание копий с изменением параметров
Позволяет создать новый объект на основе существующего, изменяя нужные параметры. Пример:
```kotlin
fun main() { val user1 = User("Alice", 25) val user2 = user1.copy(age = 30) // Создаст новый объект с другим age
println(user1) // User(name=Alice, age=25)
println(user2) // User(name=Alice, age=30)
```
🔹 Это удобно, когда объект иммутабельный (неизменяемый), но надо создать его изменённую версию. 

📌 5. componentN() — разбиение на переменные (деструктуризация)
Kotlin позволяет разбить объект на отдельные переменные с помощью componentN(). Пример:
```kotlin 
fun main() { val user = User("Alice", 25)
val (name, age) = user // Деструктуризация

println(name) // Alice
println(age)  // 25

```
🔹 Без data class пришлось бы вручную создавать getter-методы
### Ограничения data class

🔹 Должен содержать хотя бы одно свойство в первичном конструкторе
🔹 Не может быть abstract, sealed или open
🔹 Лучше всего подходит для неизменяемых данных
```kotlin 
```
📌 Нельзя унаследоваться от data class, но можно использовать sealed class:
```kotlin 
sealed class Animal 

data class Dog(val name: String) : Animal() 
data class Cat(val name: String) : Animal()
```
➡️ Это полезно, если data class входит в иерархию типов.

 📌 Как ответить на собеседовании?

📌 Что делает data class? "Data-классы в Kotlin автоматически создают методы equals(), hashCode(), toString(), copy() и componentN(). Они удобны для хранения данных, сравнения объектов и работы с коллекциями."

📌 Чем data class лучше обычного class? "Обычный класс требует переопределения equals(), hashCode() и toString(), а в data class это происходит автоматически, упрощая код."

📌 Когда НЕ стоит использовать data class? "Если класс содержит сложную логику или изменяемое состояние (Mutable State), data class не подходит."
[[kotlin]]