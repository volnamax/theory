#### Что такое AndroidManifest.xml?

[inf](https://github.com/Senchick/android-interview?tab=readme-ov-file#что-такое-androidmanifestxml)

`AndroidManifest.xml` — это файл в Android приложении, который содержит важную информацию о приложении для Android системы. Он объявляет конфигурацию приложения, включая его компоненты (активности, службы, получатели широковещательных сообщений, поставщики контента), требуемые разрешения, минимальный уровень API Android, используемый приложением, и другие настройки. `AndroidManifest.xml` читается системой при установке приложения, и на основе этой информации система знает, как взаимодействовать с приложением.


## **Зачем он нужен?**

📍 Указывает **имя пакета** приложения  
📍 Декларирует **основные компоненты** (`Activity`, `Service`, `BroadcastReceiver`, `ContentProvider`)  
📍 Определяет **разрешения** (`Permissions`), которые нужны приложению  
📍 Указывает **минимальную и целевую версию Android**  
📍 Задает **темы**, **иконки**, **запускаемый экран** и **разрешенные аппаратные возможности**

```kotlin 
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <!-- Определение минимальной и целевой версии Android -->
    <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="33" />

    <!-- Разрешения (Permissions) -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <!-- Декларация компонентов -->
    <application
        android:allowBackup="true"
        android:theme="@style/AppTheme">

        <!-- Главная Activity -->
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- Сервис -->
        <service android:name=".MyBackgroundService" />

        <!-- Broadcast Receiver -->
        <receiver android:name=".MyReceiver">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>
        </receiver>

        <!-- Content Provider -->
        <provider
            android:name=".MyContentProvider"
            android:authorities="com.example.myapp.provider"
            android:exported="false" />
        
    </application>
</manifest>

```
## **🔹 Основные элементы `AndroidManifest.xml`**

| **Тег**             | **Описание**                                                                  |
| ------------------- | ----------------------------------------------------------------------------- |
| `<manifest>`        | Корневой тег, объявляет имя пакета приложения                                 |
| `<application>`     | Определяет настройки всего приложения                                         |
| `<activity>`        | Описывает `Activity` (экраны) приложения                                      |
| `<service>`         | Декларирует `Service` (фоновый процесс)                                       |
| `<receiver>`        | Указывает `BroadcastReceiver` (слушает события системы)                       |
| `<provider>`        | Декларирует `ContentProvider` для управления данными                          |
| `<uses-permission>` | Запрашивает у пользователя доступ к разрешениям (`INTERNET`, `CAMERA` и т.д.) |
| `<uses-sdk>`        | Определяет минимальную и целевую версию API                                   |

[[android]]