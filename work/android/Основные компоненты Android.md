#### Назови все основные компоненты Android приложения? (4)

[inf](https://github.com/Senchick/android-interview?tab=readme-ov-file#назови-все-основные-компоненты-android-приложения-4)

Основные компоненты Android приложения включают:

1. **Activity** - компонент, который представляет один экран с пользовательским интерфейсом.
2. **Service** - компонент для выполнения долгосрочных операций в фоновом режиме.
3. **Broadcast Receiver** - компонент, который позволяет приложению получать и обрабатывать сообщения от других приложений или от системы. Эти сообщения могут касаться различных событий, таких как изменение состояния сети, низкий уровень заряда батареи или загрузка системы.
4. **Content Provider** - это компонент, который предоставляет доступ к данным приложения другим приложениям и компонентам. Content Providers полезны для чтения и записи данных, которые должны быть доступны другим приложениям, например, контактов, календарных событий или фотографий.


1️⃣ Activity --- это экран с пользовательским интерфейсом. Оно управляет UI и взаимодействием пользователя.

📌 Когда использовать?
✔ Экран входа
✔ Главный экран приложения
✔ Форма регистрации

📌 Пример Activity:
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```


2️⃣ Service — это компонент, который работает в фоне. Он не имеет UI и может выполнять задачи даже при свернутом приложении.

📌 Когда использовать?
✔ Фоновая загрузка данных
✔ Воспроизведение музыки
✔ Синхронизация данных

📌 Пример Service:
```kotlin
class MyService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // Выполняем фоновую работу
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
}
```

Запуск Service:
```kotlin
val intent = Intent(this, MyService::class.java)
startService(intent)
```


3️⃣ Broadcast Receiver  — это компонент, который слушает системные и пользовательские события.

📌 Когда использовать?
✔ Отслеживание включения/выключения Wi-Fi
✔ Обработка входящих SMS
✔ Реакция на низкий заряд батареи

📌 Пример BroadcastReceiver:

```kotlin
class MyReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        Toast.makeText(context, "Broadcast Received!", Toast.LENGTH_SHORT).show()
    }
}
```
Регистрация в AndroidManifest.xml:
```kotlin
<receiver android:name=".MyReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
```

4️⃣ Content Provider  позволяет делиться данными между приложениями.

📌 Когда использовать?
✔ Доступ к контактам
✔ Подключение к SQLite
✔ Работа с галереей

📌 Пример запроса данных из ContentProvider (например, контакты):
```kotlin
val cursor = contentResolver.query(ContactsContract.Contacts.CONTENT_URI, null, null, null, null)
while (cursor?.moveToNext() == true) {
    val name = cursor.getString(cursor.getColumnIndex(ContactsContract.Contacts.DISPLAY_NAME))
    println(name)
}
cursor?.close()
```

### **📌 Итоговая таблица:**

| **Компонент**         | **Что делает?**      | **Где используется?**        |
| --------------------- | -------------------- | ---------------------------- |
| **Activity**          | Экран, UI            | Экран авторизации, настройки |
| **Service**           | Фоновая работа       | Музыка, синхронизация данных |
| **BroadcastReceiver** | Обрабатывает события | Wi-Fi, заряд батареи         |
| **ContentProvider**   | Доступ к данным      | Контакты, галерея            |

🔥 Вывод:
📍 Activity — для UI
📍 Service — для фоновых задач
📍 BroadcastReceiver — для событий системы
📍 ContentProvider — для доступа к данным
[[android]]
