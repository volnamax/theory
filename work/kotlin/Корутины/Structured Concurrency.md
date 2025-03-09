#todo 
Structured Concurrency (Структурированная конкуррентность) в Kotlin Coroutines
механизм, предоставляющий иерархическую структуру для организации работы корутин

📌 Определение:
Structured Concurrency (Структурированная конкуррентность) — это подход к управлению асинхронными операциями, который гарантирует, что все запущенные корутины имеют определённый жизненный цикл и управляются из единой точки.

👉 Это означает, что корутины не работают бесконтрольно и автоматически отменяются при завершении их родительского CoroutineScope.
🔹 Зачем нужна структурированная конкуррентность?

❌ Проблема "дикого" запуска корутин
Если просто запускать корутины без CoroutineScope, их сложно контролировать, и они могут утекать.

✅ Structured Concurrency решает проблему

    Каждая корутина запускается в определённом Scope.
    Если scope отменяется, все дочерние корутины тоже отменяются.
    Код становится прозрачным, управляемым и предсказуемым.

🔹 Как работает Structured Concurrency в Kotlin?
1️⃣ CoroutineScope как основа структуры

Все корутины должны быть запущены внутри CoroutineScope.

import kotlinx.coroutines.*

fun main() = runBlocking {
    launch {
        delay(1000)
        println("Корутина завершилась")
    }
    println("Ждём завершения корутины...")
}

✅ Если runBlocking завершится — все дочерние корутины автоматически отменятся!
2️⃣ coroutineScope и supervisorScope

Они позволяют управлять жизненным циклом группы корутин.
✅ coroutineScope {} – отменяет все корутины при ошибке

suspend fun fetchData() = coroutineScope {
    launch {
        delay(1000)
        println("Данные загружены")
    }
    launch {
        delay(500)
        throw RuntimeException("Ошибка сети") // Отменит ВСЕ корутины
    }
}

🔹 Что произойдет?

    Первая корутина начнёт загрузку.
    Вторая корутина выдаст ошибку и отменит всё внутри coroutineScope.

✅ supervisorScope {} – НЕ отменяет другие корутины при ошибке

suspend fun fetchData() = supervisorScope {
    launch {
        delay(1000)
        println("Данные загружены")
    }
    launch {
        delay(500)
        throw RuntimeException("Ошибка сети") // Не отменит первую корутину
    }
}

🔹 Разница?
В supervisorScope, даже если одна корутина упала, остальные продолжают работать.
3️⃣ Job управляет жизненным циклом корутин

Когда запускается launch, он возвращает Job, который можно отменить.

val job = CoroutineScope(Dispatchers.IO).launch {
    delay(2000)
    println("Корутинa завершилась")
}

job.cancel()  // Отменит корутину до её завершения

✅ Structured Concurrency гарантирует, что корутина не "зависнет" навсегда.
4️⃣ viewModelScope и lifecycleScope в Android

В Android Structured Concurrency применяется через ViewModel и Lifecycle:
✅ viewModelScope (привязан к ViewModel)

class MyViewModel : ViewModel() {
    fun fetchData() {
        viewModelScope.launch {
            val data = getDataFromNetwork()
            println(data)
        }
    }
}

⏳ Корутины автоматически отменяются, когда ViewModel уничтожается.
✅ lifecycleScope (привязан к Activity / Fragment)

class MyActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        lifecycleScope.launch {
            delay(1000)
            println("Данные загружены")
        }
    }
}

⚡ Автоматически отменяется при onDestroy() активности.
🔹 Итог

✅ Structured Concurrency — это управление жизненным циклом корутин
✅ Гарантирует, что все корутины выполняются в определённой области (CoroutineScope)
✅ Предотвращает утечки памяти и "вечные" корутины
✅ В Android помогает управлять корутинами с viewModelScope и lifecycleScope

💡 Главное правило: Корутины должны умирать вместе с их родителем, а не висеть в памяти! 🚀


[[kotlin]]