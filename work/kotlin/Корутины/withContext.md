### `withContext` в Kotlin Coroutines

`withContext` — это **приостанавливающая** (`suspend`) функция, которая выполняет код в указанном **диспетчере** (`Dispatcher`) и **возвращает результат**.

В отличие от `launch` или `async`, **она НЕ создает новую корутину**, а просто **переключает контекст выполнения**.

📌 Синтаксис

```java
suspend fun <T> withContext(context: CoroutineContext, block: suspend () -> T): T
```

```
context → Dispatchers.Main, Dispatchers.IO, Dispatchers.Default, Dispatchers.Unconfined.
block → Код, который нужно выполнить.
Возвращает значение (T).

```

Пример использования withContext

Предположим, у нас есть функция, загружающая данные из сети и обновляющая UI.

```java
import kotlinx.coroutines.*

fun main() = runBlocking {
    println("Запуск на потоке: ${Thread.currentThread().name}")

    val result = withContext(Dispatchers.IO) { // Переключение на IO-поток
        println("Загрузка данных на потоке: ${Thread.currentThread().name}")
        delay(1000) // Симуляция сетевого запроса
        "Данные загружены"
    }

    println("Обновление UI на потоке: ${Thread.currentThread().name}")
    println(result)
}

```

- Код начался в `main` потоке.
- `withContext(Dispatchers.IO) {}` переключил выполнение на IO-поток для сетевого запроса.
- После завершения `withContext` вернул выполнение в `main`

Нельзя использовать withContext ВНУТРИ launch {} без необходимости

Плохо:

```java
launch {
withContext([Dispatchers.IO](<http://dispatchers.io/>)) {
// Долгая операция
	}
}
```

⛔ Почему?

```
launch {} уже запускает корутину, withContext лишь переключает контекст.
Лучше сразу запускать launch(Dispatchers.IO) {}!

```

Хорошо:

```java
launch([Dispatchers.IO](<http://dispatchers.io/>)) {
// Долгая операция
}
```

🎯 Итог

✔ withContext используется для смены контекста внутри корутины. 
✔ Не создает новую корутину, а переключает контекст и ждет выполнения блока.
✔ Возвращает результат выполнения. 
✔ Отлично подходит для переключения между потоками (например, IO → Main).

🔥 Как ответить на собеседовании? "withContext — это suspend-функция, которая переключает поток выполнения внутри корутины и возвращает результат. Она не создает новую корутину, а используется, когда нужно выполнить код в другом потоке, например, запрос в [Dispatchers.IO](http://dispatchers.io/), а затем обновить UI в Dispatchers.Main. [[Dispatchers]]

[[kotlin]]