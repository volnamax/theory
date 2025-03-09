### AssociateBy - List to Map в Kotlin

- это функция коллекций, которая создает МАР из ЛИСТ, используя предоставленную функцию для определения ключей.

**Пример: `List` → `Map` (ключ – `id`)**

```kotlin
data class User(val id: Int, val name: String)

val users = listOf(
    User(1, "Alice"),
    User(2, "Bob"),
    User(3, "Charlie")
)

// Создаём Map, где ключ — `id`
val userMap = users.associateBy { it.id }

println(userMap)
//{1=User(id=1, name=Alice), 2=User(id=2, name=Bob), 3=User(id=3, name=Charlie)}
```

3️⃣ associateBy с distinct (если есть дубликаты)

Если список содержит дубликаты, в Map останется последний элемент.

✅ associateBy – когда ключ уникален (иначе дубликаты перезапишутся). 
✅ groupBy – когда у одного ключа может быть несколько значений.

[[kotlin]]