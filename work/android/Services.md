**Service** – это компонент приложения, который может выполнять длительные операции в фоновом режиме и не предоставляет пользовательского интерфейса. Другими словами, это способ выполнить некоторые действия в фоне, например, проигрывание музыки, загрузку файлов и т. д., без взаимодействия с пользовательским интерфейсом. Сервисы могут быть как связанными (bound), так и несвязанными. Связанный сервис предлагает клиент-серверный интерфейс, который позволяет компонентам (например, активности) взаимодействовать с сервисом, отправлять запросы, получать результаты через IPC (Межпроцессное взаимодействие). Несвязанный сервис обычно выполняется на заднем плане и не связан с каким-либо пользовательским интерфейсом или компонентом, который его запустил.

## **1️⃣ Виды `Service` в Android**

### 🔹 **Несвязанный (`Started`)**

📌 Используется, когда сервис **начинается и работает независимо**, даже если вызывающий компонент (например, `Activity`) уничтожен.

✅ **Ключевые моменты:**

- Запускается с помощью `startService(Intent)`.
- Не связан с `Activity` и не может взаимодействовать с ним напрямую.
- Работает до вызова `stopSelf()` или `stopService(Intent)`.
- Например, **загрузка файлов, музыка в фоне, синхронизация данных.**

📌 **Пример:**

```kotlin 
class MyService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            // Долгая фоновая операция (пример загрузки файла)
            downloadFile()
            stopSelf() // Останавливаем сервис после завершения работы
        }.start()
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? = null
}
**Запуск сервиса:**
val intent = Intent(this, MyService::class.java)
startService(intent)

```

### **Связанный (`Bound`)**

📌 Используется, когда `Service` **должен взаимодействовать с `Activity`**, передавая данные и выполняя запросы в реальном времени.

✅ **Ключевые моменты:**

- Запускается с помощью `bindService(Intent, ServiceConnection, flags)`.
- Позволяет `Activity` или `Fragment` взаимодействовать с `Service`.
- Останавливается автоматически, если нет клиентов, использующих `unbindService()`.
- Например, **музыкальный плеер, взаимодействие с Bluetooth, API-запросы.**
```kotlin 
class BoundService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun getService(): BoundService = this@BoundService
    }

    override fun onBind(intent: Intent?): IBinder = binder

    fun getRandomNumber(): Int = (1..100).random()
}

```

📌 **Связывание с `Activity`:**

```kotlin 
class MainActivity : AppCompatActivity() {
    private var service: BoundService? = null
    private var isBound = false

    private val connection = object : ServiceConnection {
        override fun onServiceConnected(name: ComponentName?, binder: IBinder?) {
            val localBinder = binder as BoundService.LocalBinder
            service = localBinder.getService()
            isBound = true
        }

        override fun onServiceDisconnected(name: ComponentName?) {
            isBound = false
        }
    }

    override fun onStart() {
        super.onStart()
        val intent = Intent(this, BoundService::class.java)
        bindService(intent, connection, Context.BIND_AUTO_CREATE)
    }

    override fun onStop() {
        super.onStop()
        if (isBound) {
            unbindService(connection)
            isBound = false
        }
    }
}

```

## **2️⃣ Foreground Service (`Сервис на переднем плане`)**

📌 `Foreground Service` — это сервис, который **работает в фоне, но отображает уведомление**, чтобы пользователь знал, что он активен.

✅ **Используется для:**

- Проигрывания музыки.
- Отслеживания GPS.
- Фоновой записи аудио.
- Запуска длительных операций.

📌 **Пример:**


```kotlin
class ForegroundService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val notification = NotificationCompat.Builder(this, "channel_id")
            .setContentTitle("Foreground Service")
            .setContentText("Сервис запущен")
            .setSmallIcon(R.drawable.ic_launcher_foreground)
            .build()

        startForeground(1, notification)  // Запускаем Foreground Service

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? = null
}

**Запуск Foreground Service:**
val intent = Intent(this, ForegroundService::class.java)
startService(intent)




```

## **Разница между `Started`, `Bound` и `Foreground` Service**

|Тип `Service`|Как запускается|Когда останавливается|Где используется|
|---|---|---|---|
|**Started Service**|`startService(intent)`|`stopSelf()` или `stopService(intent)`|Фоновые задачи (загрузка файлов, API-запросы)|
|**Bound Service**|`bindService(intent, connection, flags)`|`unbindService(connection)`|Клиент-серверное взаимодействие (музыкальный плеер, Bluetooth)|
|**Foreground Service**|`startForeground(id, notification)`|Когда приложение явно останавливает его|GPS, музыка, работа с сенсорами|
#### Что такое `JobScheduler`?

	
**JobScheduler** – это системный сервис, доступный начиная с Android 5.0 (API уровень 21), который позволяет разработчикам планировать различные виды задач (работы) для выполнения в определенных условиях. С помощью JobScheduler можно управлять выполнением задач в зависимости от таких условий, как состояние сети, заряд батареи и другие. Это позволяет приложениям выполнять фоновые задачи более эффективно, экономя заряд батареи и ресурсы системы. Работы, запланированные с помощью JobScheduler, могут выполняться даже после перезагрузки устройства.

📌 **Как запланировать задачу?**
```kotlin 
class MyJobService : JobService() {
    override fun onStartJob(params: JobParameters?): Boolean {
        // Фоновая задача (например, синхронизация данных)
        Thread {
            Log.d("JobService", "Фоновая работа выполняется...")
            jobFinished(params, false) // Завершаем работу
        }.start()
        return true // Возвращаем true, если задача выполняется в фоне
    }

    override fun onStopJob(params: JobParameters?): Boolean {
        return false // false = задача НЕ перезапускается при сбое
    }
}
	
```
📌 Запланировать выполнение задачи
```kotlin
val jobScheduler = getSystemService(JobScheduler::class.java)

val jobInfo = JobInfo.Builder(1, ComponentName(this, MyJobService::class.java))
    .setRequiredNetworkType(JobInfo.NETWORK_TYPE_UNMETERED) // Только Wi-Fi
    .setRequiresCharging(true) // Только при зарядке
    .setPeriodic(15 * 60 * 1000) // Повторять каждые 15 минут
    .build()

jobScheduler.schedule(jobInfo)
```
📌 Отменить задачу
```kotlin 
jobScheduler.cancel(1) // Отменить задачу с ID 1
```




[[android]]