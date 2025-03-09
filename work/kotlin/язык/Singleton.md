### **Как создать Singleton класс в Kotlin?**

- Для создания синглтона в Kotlin используется `object`.
```kotlin
object DatabaseManager {
    fun connect() = println("Подключение к БД...")
}

fun main() {
    DatabaseManager.connect() // Подключение к БД...
}

```


### **2️⃣ Использование `companion object` (для Singleton внутри класса)**

Если вам нужен **Singleton внутри класса**, используйте `companion object`
```kotlin
class Logger private constructor() {
    companion object {
        val instance: Logger by lazy { Logger() }
    }

    fun log(message: String) = println("Log: $message")
}

fun main() {
    Logger.instance.log("Запуск приложения") // Log: Запуск приложения
}

```
[[kotlin]]   [[companion]]
