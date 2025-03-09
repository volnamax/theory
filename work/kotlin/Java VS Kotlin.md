
| **Функция**         | **Java**                              | **Kotlin**                                   |
| ------------------- | ------------------------------------- | -------------------------------------------- |
| Код                 | Длинный, много `boilerplate`          | Короткий и лаконичный                        |
| Null Safety         | Нет встроенной защиты от `null`       | `NullPointerException` исключен (`?` и `!!`) |
| Coroutines          | Потоки (`Thread`, `Executor`)         | **Корутины (`suspend`, `Flow`)**             |
| Extension Functions | Нет                                   | ✅ `fun String.myExtension() {...}`           |
| Data Classes        | `getters/setters`, `equals()` вручную | ✅ `data class` (автоматически)               |
| Типизация           | `Object obj = "text";` (нестрого)     | ✅ **Четкая типизация**                       |
| Android Support     | Да                                    | ✅ **Официальный язык Android**               |

📌 **Java:** Нет `NullPointerException

```jsx
String name = null;
System.out.println(name.length()); // NPE
```

📌 **Kotlin (защита от `null`)**

```kotlin
var name: String? = null // `?` позволяет `null`
println(name?.length) // null-safe
println(name!!.length) // Форсированный вызов (может упасть)
```

**❌ 3. Extension Functions**

📌 **Java (нельзя добавить метод в `String`)**

```java
public class StringUtils {
    public static String capitalize(String text) {
        return text.toUpperCase();
    }
}
```

📌 **Kotlin (можно добавить метод к `String`)**

```kotlin
fun String.capitalizeText(): String = this.uppercase()

println("hello".capitalizeText()) // "HELLO"
```

**❌ 4. Корутины вместо потоков**

📌 **Java (потоки сложные и тяжелые)**

```java
new Thread(() -> {
    System.out.println("Выполняется в фоне");
}).start();
```

📌 **Kotlin (корутины легкие)**

```java
GlobalScope.launch {
    println("Выполняется в фоне")
}
```

✅ Kotlin безопаснее (null safety)
✅ Код короче и читабельнее
✅ Extension functions делают код удобнее
✅ Корутины заменяют потоки 
✅ Kotlin — официальный язык Android
### **📌 Как ответить на собеседовании?**

📌 **Как Kotlin совместим с Java?**  
_"Kotlin полностью совместим с Java — можно вызывать Java-код из Kotlin и наоборот. Это позволяет использовать существующие Java-библиотеки и плавно мигрировать проекты."_

📌 **Во что компилируется Kotlin?**  
_"Kotlin может компилироваться в байт-код JVM, JavaScript (Kotlin/JS) и исполняемый код для iOS и десктопа (Kotlin/Native)."_

📌 **Почему Kotlin предпочтительнее для Android?**  
_"Он безопаснее (Null Safety), более лаконичный, поддерживает Coroutines и официально рекомендован Google."_




[[kotlin]]
