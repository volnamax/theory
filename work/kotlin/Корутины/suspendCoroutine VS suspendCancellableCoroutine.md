### `suspendCoroutine` vs `suspendCancellableCoroutine` в Kotlin

Эти функции позволяют приостановить выполнение корутины и продолжить её позже вручную, когда появятся данные. Они полезны для интеграции корутин с API, основанными на колбэках.

Это низкоуровневая функция, которая приостанавливает корутину до вызова переданного колбэка. ✅ Как работает?

```
Корутинная функция замораживается (suspend) на suspendCoroutine.
Ожидает, пока колбэк вызовет resume() или resumeWithException().
После вызова корутина продолжается с переданным результатом.

```

✅ Пример: Интеграция с API на колбэках

Допустим, у нас есть API:

```java
fun loadDataFromApi(callback: (String) -> Unit) {
	Thread.sleep(1000) // Имитация задержки
	callback("Данные загружены!")
}
```

Теперь сделаем его совместимым с корутинами:

```java
import kotlinx.coroutines.*
import kotlin.coroutines.suspendCoroutine
suspend fun loadDataAsync(): String = suspendCoroutine { continuation ->
	loadDataFromApi { result ->
		continuation.resume(result) // Продолжаем корутину с результатом
	}
}

fun main() = runBlocking {
	val data = loadDataAsync()
	println(data) // Данные загружены!
}

```

🔹 Что происходит?

```
suspendCoroutine приостанавливает корутину.
loadDataFromApi вызывает колбэк через 1 секунду.
Колбэк вызывает resume(), и корутина продолжается.

```

📌 2. suspendCancellableCoroutine

Это расширенная версия suspendCoroutine, которая поддерживает отмену корутины. ✅ Почему это важно?

```
Если корутина отменяется, suspendCancellableCoroutine позволяет выполнить дополнительные действия (например, освободить ресурсы).
В suspendCoroutine отмена корутины не останавливает ожидающий колбэк, что может вызвать утечки памяти.

```

✅ Пример с отменой

Допустим, у нас есть API с возможностью отмены:

```java
class ApiRequest {
var isCancelled = false
fun fetchData(callback: (String) -> Unit) {
    Thread.sleep(1000) // Имитация задержки
    if (!isCancelled) callback("Данные загружены!")
}

fun cancel() {
    isCancelled = true
}

```

Теперь добавим поддержку отмены в корутина:

```java
import kotlin.coroutines.suspendCancellableCoroutine

suspend fun loadDataCancellable(apiRequest: ApiRequest): String =
    suspendCancellableCoroutine { continuation ->
        apiRequest.fetchData { result ->
            if (continuation.isActive) {
                continuation.resume(result) // Завершаем корутину
            }
        }

        continuation.invokeOnCancellation {
            apiRequest.cancel() // Отмена запроса
            println("Запрос отменен!")
        }
    }

fun main() = runBlocking {
    val apiRequest = ApiRequest()

    val job = launch {
        println(loadDataCancellable(apiRequest))
    }

    delay(500) // Отмена через 0.5 сек
    job.cancel()
}

```

🔹 Что произошло?

```
suspendCancellableCoroutine ждет ответа от fetchData().
Если корутина отменяется раньше, вызывается invokeOnCancellation, и apiRequest.cancel() останавливает операцию.
Без suspendCancellableCoroutine запрос продолжил бы выполняться даже после отмены корутины.

```

[[kotlin]]