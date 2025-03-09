**BroadcastReceiver** — это компонент приложения, который позволяет реагировать на широковещательные сообщения от других приложений или от самой системы. Эти сообщения могут касаться различных событий, таких как изменение заряда батареи, получение SMS или изменение сетевого состояния.
#### Какой компонент необязательно указывать в манифесте?

**Broadcast Receiver** - обычно необходимо объявлять, но существует возможность динамической регистрации (в коде, а не в манифесте) для некоторых случаев использования.

## 📌 Какой компонент необязательно указывать в манифесте?

Из основных компонентов Android (Activity, Service, BroadcastReceiver, ContentProvider) необязательно указывать в AndroidManifest.xml:

📍 BroadcastReceiver, если он регистрируется динамически в коде.
🔹 Что значит "динамическая регистрация"?

Вместо объявления BroadcastReceiver в манифесте:
```kotlin
<receiver android:name=".MyReceiver">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
```


мы можем зарегистрировать его в коде с помощью registerReceiver():
```kotlin
val receiver = MyReceiver()
val intentFilter = IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED)

// Регистрируем приемник
registerReceiver(receiver, intentFilter)

// Отменяем регистрацию (например, при закрытии Activity)
unregisterReceiver(receiver)
```


🔹 Когда использовать динамическую регистрацию?

✅ Когда BroadcastReceiver нужен только во время работы приложения
✅ Когда не нужен после закрытия Activity или Fragment
✅ Когда слушаются события только внутри приложения (например, передача данных между компонентами)
🔹 Когда BroadcastReceiver НУЖНО указывать в AndroidManifest.xml?

❗ Если BroadcastReceiver должен работать даже если приложение закрыто, например:
✔ BOOT_COMPLETED (событие перезагрузки устройства)
✔ CONNECTIVITY_CHANGE (изменение сети)
✔ BATTERY_LOW (низкий заряд батареи)

В этих случаях динамическая регистрация не подойдет, так как приемник будет уничтожен после закрытия приложения.
📌 Итог

🔹 Activity, Service, ContentProvider всегда нужно объявлять в AndroidManifest.xml.
🔹 BroadcastReceiver можно регистрировать динамически в коде без манифеста, но только если он нужен только во время работы приложения. 🚀
[[android]]