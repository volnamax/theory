#### `flowOn`, Dispatchers

[](https://github.com/Senchick/android-interview?tab=readme-ov-file#flowon-dispatchers)

**flowOn** позволяет изменить контекст выполнения операций внутри потока (Flow). Это особенно полезно, когда тяжелые операции должны выполняться в фоновом потоке, а результаты обрабатываться в основном потоке

```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun main() = runBlocking {
  flow {
      for (i in 1..3) {
          Thread.sleep(100) // Имитация длительной операции
          emit(i)
      }
  }.flowOn(Dispatchers.Default) // Выполнение в фоновом потоке
  .map { value ->
      "Преобразованное значение $value"
  }
  .collect { value ->
      println("$value на потоке ${Thread.currentThread().name}")
  }
}
```

[[kotlin]] [[Flow]]