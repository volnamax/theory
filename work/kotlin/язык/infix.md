позволяет вызывать функции с одним параметром без скобок и точки.

```kotlin
infix fun Int.add(other: Int): Int = this + other
val sum = 1 add 2
```
[[kotlin]]