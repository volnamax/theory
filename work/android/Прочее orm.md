### **1️⃣ Что такое ORM? Как это работает?**

**ORM (Object-Relational Mapping)** — это способ работы с базами данных, при котором объекты классов связываются с таблицами базы данных.

**Как работает ORM?**

- Таблицы базы данных отображаются на **объекты**.
- **Запросы** к базе данных заменяются вызовами методов.
- ORM **упрощает работу** с БД, не требуя написания SQL-запросов.

**Примеры ORM в Android:**

- **Room (Android Jetpack)**
- **Realm**
- **ObjectBox**

**Пример работы с Room:**
```kotlin
@Entity
data class User(
    @PrimaryKey val id: Int,
    val name: String
)

@Dao
interface UserDao {
    @Query("SELECT * FROM User")
    fun getAll(): List<User>

    @Insert
    fun insert(user: User)
}

@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}

```

### **2️⃣ Как сохранить состояние Activity при повороте экрана?**

При повороте экрана активность **пересоздается**, поэтому нужно **сохранять данные**.

#### **Способы сохранения состояния:**

1️⃣ **ViewModel** (рекомендуется)
```kotlin
class MyViewModel : ViewModel() {
    var count = 0
}
```

### **3️⃣ Какие способы хранения данных в Android?**

📌 В Android есть несколько способов хранения данных:

1️⃣ **SharedPreferences** → хранение небольших данных (настроек)  
2️⃣ **Room (SQLite)** → для хранения структурированных данных  
3️⃣ **Files (Internal/External Storage)** → хранение файлов  
4️⃣ **DataStore** → замена SharedPreferences  
5️⃣ **ContentProvider** → обмен данными между приложениями  
6️⃣ **Cloud (Firebase, API)** → синхронизация с сервером

---
### **4️⃣ Объясните Scoped Storage в Android**

**Scoped Storage** – новая система хранения файлов в Android 10+ для защиты данных пользователя.

**Изменения:**

- Приложение **не может** видеть файлы других приложений.
- Для доступа к файлам **нужны разрешения** (`MediaStore`, `SAF`).
- **Внутренний кэш** не требует разрешений.

**Пример запроса разрешения:**
`requestPermissions(arrayOf(Manifest.permission.READ_EXTERNAL_STORAGE), 1)`

### **5️⃣ Как зашифровать данные в Android?**

📌 Для защиты данных используется **AES, RSA, EncryptedSharedPreferences**.

**1️⃣ EncryptedSharedPreferences** (Jetpack Security)
```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val sharedPrefs = EncryptedSharedPreferences.create(
    context,
    "secure_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
sharedPrefs.edit().putString("token", "secret").apply()

```
### **6️⃣ Разница между `commit()` и `apply()` в SharedPreferences**

**SharedPreferences** используется для хранения **настроек** приложения.

✔ `commit()` → **синхронно** записывает данные, возвращает `true/false`.  
✔ `apply()` → **асинхронно** записывает данные, не блокируя поток.
[[android]]