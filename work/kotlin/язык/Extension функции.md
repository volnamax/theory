### **38. Extension функции**

**Extension functions** (расширения) позволяют добавлять новые методы к существующим классам **без их изменения**.

Как работают extension функции?

```kotlin
fun String.addExclamation(): String {
		return this + "!"
}
```

```kotlin
val message = "Hello"
println(message.addExclamation()) // Hello!
```
[[kotlin]]