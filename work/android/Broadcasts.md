	### **2️⃣ Broadcasts: широковещательные сообщения**

**Broadcasts** — это механизм передачи сообщений между компонентами Android или между разными приложениями. Они используются для уведомления об изменениях в системе, например:

- Изменение состояния батареи 🔋
- Смена режима сети (Wi-Fi/Mobile) 📶
- Уведомления от сторонних приложений 📩
### **📌 Виды Broadcasts**

1. **Статические Broadcast Receivers** (указываются в `AndroidManifest.xml`).
2. **Динамические Broadcast Receivers** (регистрируются в коде).
### **3️⃣ BroadcastReceiver: приёмник широковещательных сообщений**

BroadcastReceiver — это компонент, который слушает **Broadcasts** и реагирует на них.

✅ **Пример статического BroadcastReceiver для отслеживания состояния сети** 📌 (Нужно объявить в `AndroidManifest.xml`):

```xml
<receiver android:name=".MyReceiver">
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
    </intent-filter>
</receiver>

```
📌 **Код BroadcastReceiver (`MyReceiver.kt`):**
```kotlin
class MyReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == "android.net.conn.CONNECTIVITY_CHANGE") {
            Toast.makeText(context, "Сеть изменилась!", Toast.LENGTH_SHORT).show()
        }
    }
}

```
✅ **Пример динамического BroadcastReceiver (регистрируется в коде)**
```kotlin
val receiver = object : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Toast.makeText(context, "Получен Broadcast!", Toast.LENGTH_SHORT).show()
    }
}

override fun onResume() {
    super.onResume()
    registerReceiver(receiver, IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED))
}

override fun onPause() {
    super.onPause()
    unregisterReceiver(receiver)
}

```

4️⃣ Липкие (Sticky) Intents

Иногда нужно, чтобы BroadcastReceiver мог получать предыдущие события. Для этого используется Sticky Intent.

✅ Пример получения текущего состояния батареи с помощью Sticky Intent:

val intent = registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
val level = intent?.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) ?: -1
Log.d("Battery", "Уровень заряда: $level%")

5️⃣ Ordered Broadcasts (упорядоченные broadcast'ы)

Ordered Broadcasts позволяют передавать данные по цепочке от одного BroadcastReceiver к другому.

✅ Пример отправки Ordered Broadcast

sendOrderedBroadcast(Intent("com.example.CUSTOM_ACTION"), null)

6️⃣ LocalBroadcastManager (устарел, но ещё используется)

LocalBroadcastManager использовался для передачи сообщений внутри одного приложения, но сейчас рекомендуется использовать LiveData, Flow или EventBus.

✅ Пример с LocalBroadcastManager

val intent = Intent("custom-event")
intent.putExtra("message", "Привет, Broadcast!")
LocalBroadcastManager.getInstance(context).sendBroadcast(intent)
### **Итог**

| **Механизм**        | **Назначение**                                      |
| ------------------- | --------------------------------------------------- |
| `Explicit Intent`   | Запуск конкретного компонента (Activity, Service)   |
| `Implicit Intent`   | Запуск действия без указания конкретного компонента |
| `BroadcastReceiver` | Получение сообщений от системы или приложений       |
| `Sticky Intent`     | Получение последнего отправленного интента          |
| `Ordered Broadcast` | Упорядоченная обработка сообщений                   |
[[android]]