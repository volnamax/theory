### Ключевое слово Open в Kotlin

В Kotlin все классы по умолчанию закрыты (final) для наследования. 
Чтобы класс мог быть унаследован, он должен быть помечен как open.

**2️⃣ `open class` – разрешает наследование**

```kotlin
// Без `open` наследование запрещено
class Animal

class Dog : Animal() // Ошибка: Class 'Animal' is final

// С `open` наследование разрешено
open class Animal

class Dog : Animal() // ✅ Теперь все работает
```

**`open fun` – разрешает переопределение метода**

```kotlin
open class Animal {
    open fun makeSound() {
        println("Some generic animal sound")
    }
}

class Dog : Animal() {
    override fun makeSound() {
        println("Bark! Bark!")
    }
}

fun main() {
    val dog = Dog()
    dog.makeSound() // "Bark! Bark!"
}
```

|**Ключевое слово**|**Что делает?**|
|---|---|
|**`final` (по умолчанию)**|❌ Запрещает наследование|
|**`open`**|✅ Разрешает наследование|
|**`open fun`**|✅ Разрешает переопределение метода|
|**`final fun`**|❌ Запрещает переопределение метода|
[[kotlin]]