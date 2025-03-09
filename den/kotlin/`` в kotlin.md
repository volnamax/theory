[[kotlin]]

Используется как обертка над ключевыми словами для избежания конфликтов c *Java*. Например *System.`in`*.
Также можно издеваться над синтаксисом:

```kotlin
fun main(){  
	val `2` = 5  
	println(`2`)  
}
```

Будет выведено, как ни странно, 5

[stackoverflow](https://stackoverflow.com/questions/48152722/kotlin-what-is-the-character-for)