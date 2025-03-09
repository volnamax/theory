#kotlin #прога #random 
[[kotlin]]

```kotlin
fun main() {
	val diceRange: IntRange = 1..6
    val randomNumber = diceRange.random()
    println(randomNumber)
}
```

> Тип данных *IntRange* позволяет создавать диапазоны целых чисел (отрицательных тоже).

Можно применять методы сразу к последовательностям:

```kotlin
(-5..+10).random()
```

[developer.android.com](https://developer.android.com/codelabs/basic-android-kotlin-training-create-dice-roller-in-kotlin?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-kotlin-four%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-training-create-dice-roller-in-kotlin#1)
