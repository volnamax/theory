#### Что такое `Context`? Зачем он используется?

[inf](https://github.com/Senchick/android-interview?tab=readme-ov-file#что-такое-context-зачем-он-используется)
**контекст** - контекст текущего состояния приложения или объекта. позволяет новосозданным объектам понять, что вообще происходит.
также контекст - проводник в систему:
*получает доступ к ресурсам/бд/преференсам*

**контекст приложения** - синглтон, привязан к жц приложения.
если при создании синглтона передать контекст активити, а не приложения, то произойдет утечка памяти, т.к. синглтон сохранит ссылку на активити и она не будет уничтожена гк.

наследники контекста:
- [[activity]]
- [[service]]
- [[application]]

почему [[broadcast receiver]] не наследуется от контекста? потому что он не нужен, его задача принять сообщение и куда-нибудь редиректнуть


`Context` в Android — это класс, который предоставляет доступ к глобальной информации о приложении. Он используется для получения доступа к ресурсам, файловым системам, вызова активностей и служб. Существует три типа контекста: `ApplicationContext`, `ActivityContext`, `BaseContext`.

1. `ApplicationContext` связан с жизненным циклом приложения
2. `ActivityContext` связан с жизненным циклом активности.
3. `BaseContext` в Android является базовым классом контекста (`Context`). Он используется как родительский класс для `ActivityContext` и `ApplicationContext`, предоставляя основные функции контекста, которые могут быть расширены или использованы в этих специализированных контекстах. `BaseContext` обеспечивает доступ к ресурсам и системным службам, таким как запуск активностей, взаимодействие с различными сервисами Android и управление ресурсами приложения. В контексте Android разработки, когда говорят о `BaseContext`, чаще всего имеют в виду фундаментальную функциональность контекста, на которой основаны все другие типы контекстов.

Context необходим для:

1. Доступа к базовым функциям приложения, таким как доступ к ресурсам, базам данных и настройкам.
2. Запуска других компонентов, таких как активности и службы.
3. Взаимодействия с другими приложениями.


## Что такое Context в Android?

Context в Android — это интерфейс, предоставляющий доступ к системным ресурсам, службам и управлению жизненным циклом компонентов приложения.
📌 Зачем нужен Context?

🔹 Доступ к ресурсам приложения

    getResources(): Получение строк, изображений, размеров и других ресурсов.

`val appName = context.getString(R.string.app_name)

🔹 Работа с файлами и настройками

    getSharedPreferences(): Доступ к настройкам пользователя.
    openFileInput() / openFileOutput(): Чтение и запись файлов.

val prefs = context.getSharedPreferences("my_prefs", Context.MODE_PRIVATE)

🔹 Запуск новых Activity или Service

    startActivity(Intent(context, MainActivity::class.java))
    startService(Intent(context, MyService::class.java))

🔹 Работа с базой данных

    context.getDatabasePath("my_database")
    Room.databaseBuilder(context, MyDatabase::class.java, "database-name").build()

🔹 Обращение к системным сервисам

    getSystemService(Context.CONNECTIVITY_SERVICE): Получение сервисов Android.

val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager

## **Итоговая таблица по видам `Context`**

|Тип `Context`|Жизненный цикл|Где использовать?|Где НЕ использовать?|
|---|---|---|---|
|**Application Context**|Живет столько же, сколько приложение|**Синглтоны, базы данных, SharedPreferences, сервисы**|**Работа с UI, `Toast`, `Dialog`**|
|**Activity Context**|Привязан к `Activity`, уничтожается вместе с ним|**Работа с UI (диалоги, навигация, Toast, ViewBinding)**|**В `Singleton` (утечки памяти)**|
|**Base Context**|Родительский контекст, общий для всех|**В `Service`, `BroadcastReceiver`**|**Нельзя использовать для UI**|


1️⃣ Application Context
📌 Что это?

    Application Context существует столько же, сколько живет приложение.
    Он не привязан к какому-либо Activity или Fragment.
    Используется, когда нужен Context, который не изменится при смене активностей.

📌 Когда использовать?

✔ Работа с глобальными ресурсами:
✔ Инициализация библиотек (например, Glide, Retrofit, Room)
✔ Работа с базами данных, SharedPreferences
✔ Запуск сервисов
❌ Нельзя использовать для UI элементов (например, Toast, Dialog)
📌 Пример использования Application Context
✅ Инициализация SharedPreferences (Глобальное хранилище)

```kotlin
val sharedPrefs = applicationContext.getSharedPreferences("app_prefs", Context.MODE_PRIVATE)
sharedPrefs.edit().putString("key", "value").apply()
```

✅ Запуск службы (Service)

val intent = Intent(applicationContext, MyBackgroundService::class.java)
applicationContext.startService(intent)

✅ Передача в Singleton (Во избежание утечек памяти)
```kotlin
class MySingleton private constructor(private val appContext: Context) {
    companion object {
        @Volatile private var instance: MySingleton? = null
        fun getInstance(context: Context) =
            instance ?: synchronized(this) {
                instance ?: MySingleton(context.applicationContext).also { instance = it }
            }
    }
}
```


🔴 ⚠ Ошибка: Нельзя использовать applicationContext для Toast

Toast.makeText(applicationContext, "Привет!", Toast.LENGTH_SHORT).show() // ❌ Может вызвать ошибку

✅ Используем Activity Context

Toast.makeText(this, "Привет!", Toast.LENGTH_SHORT).show() // ✅

2️⃣ Activity Context
📌 Что это?

    Activity Context привязан к Activity.
    Он существует, пока Activity не уничтожится.
    Используется для работы с UI, Toast, Dialog, Navigation, ViewBinding.

📌 Когда использовать?

✔ Создание Toast и Dialog
✔ Открытие новых Activity
✔ Работа с View, RecyclerView, Navigation
❌ Не использовать в Singleton (чтобы избежать утечек памяти)
📌 Пример использования Activity Context
✅ Запуск новой Activity

val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)

✅ Создание Toast

Toast.makeText(this, "Привет!", Toast.LENGTH_SHORT).show()

✅ Диалоговые окна

AlertDialog.Builder(this)
    .setTitle("Внимание!")
    .setMessage("Вы уверены?")
    .setPositiveButton("Да") { _, _ -> }
    .setNegativeButton("Нет", null)
    .show()

🔴 ⚠ Ошибка: Не передавай Activity Context в Singleton, это вызовет утечку памяти!

class MySingleton private constructor(val context: Context) { } // ❌

✅ Используй applicationContext

```kotlin
class MySingleton private constructor(private val appContext: Context) {
    companion object {
        @Volatile private var instance: MySingleton? = null
        fun getInstance(context: Context) =
            instance ?: synchronized(this) {
                instance ?: MySingleton(context.applicationContext).also { instance = it }
            }
    }
}
```


3️⃣ Base Context
📌 Что это?

    Base Context – это родительский контекст для Activity и Application.
    Используется внутри Service, BroadcastReceiver, ViewModel для доступа к ресурсам.

📌 Когда использовать?

✔ Работа в Service (фоновая работа)
✔ BroadcastReceiver (прием событий)
✔ Доступ к ресурсам без привязки к Activity
❌ Не использовать напрямую для UI, потому что он не всегда доступен в Activity
📌 Пример использования Base Context
✅ Работа в Service
```kotlin
class MyService : Service() {
    override fun onCreate() {
        super.onCreate()
        val sharedPrefs = baseContext.getSharedPreferences("service_prefs", Context.MODE_PRIVATE)
    }
}
```


✅ В BroadcastReceiver

```kotlin
class MyReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val sharedPrefs = context.getSharedPreferences("receiver_prefs", Context.MODE_PRIVATE)
    }
}

```

🔴 ⚠ Ошибка: Нельзя использовать Base Context напрямую для UI.

val toast = Toast(baseContext) // ❌ Ошибка

✅ Используем Activity Context

Toast.makeText(activity, "Привет!", Toast.LENGTH_SHORT).show() // ✅
[[android]]