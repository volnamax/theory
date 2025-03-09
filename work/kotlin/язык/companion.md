### **Что такое companion object в Kotlin?**

**Companion object** – это специальный объект внутри класса, который позволяет объявлять **статические переменные и методы**, доступные без создания экземпляра класса

✔ Аналог `static` в Java  
✔ Может содержать переменные и функции  
✔ Может реализовывать интерфейсы

`companion object` – это **singleton-объект**, объявленный внутри класса. Он позволяет **создавать методы и переменные, доступные без создания экземпляра класса**, аналогично `static` в Java.

**Эквивалент `static` в Java**
- Позволяет объявлять переменные и методы, доступные без создания экземпляра класса.
```kotlin
class Example {
    companion object {
        val CONSTANT = "Константа"
        fun printHello() = println("Hello, world!")
    }
}

fun main() {
    println(Example.CONSTANT) // ✅ Вывод: Константа
    Example.printHello()      // ✅ Вывод: Hello, world!
}
```
2️⃣ **Может реализовывать интерфейсы**

- В отличие от `static` методов в Java, `companion object` может **имплементировать интерфейсы**.
```kotlin 
interface Printable {
    fun print()
}

class Example {
    companion object : Printable {
        override fun print() {
            println("Companion object печатает...")
        }
    }
}

fun main() {
    Example.print() // ✅ Вывод: Companion object печатает...
}

```
3️⃣ **Можно передавать как объект (например, в функции)**
- `companion object` – это полноценный объект, его можно передавать в другие функции.
4️⃣ **Lazy инициализация**
- Переменные `companion object` инициализируются **только при первом обращении**, что **экономит память**.
### **Что является эквивалентом статических методов Java в Kotlin?**

В Kotlin нет прямого эквивалента статических методов Java, но можно использовать companion object или объектные объявления (object) для создания функций, доступных без экземпляра класса. [[companion]]

📝 _"Как создать статический метод в Kotlin?"_  
✅ _"Использовать companion object или object."_


[[kotlin]]