### **Когда использовать запечатанные классы (sealed classes) Kotlin?**

**Запечатанные классы (`sealed class`) используются, когда нужно ограничить количество возможных подклассов**. Они позволяют создать **закрытую иерархию классов**, которую нельзя расширить за пределами файла, где она объявлена.

Когда у вас ограниченное множество возможных состояний

Пример: Состояния загрузки данных

```kotlin
sealed class UiState {
		object Loading : UiState()
		data class Success(val data: String) : UiState()
		data class Error(val message: String) : UiState()
}
```

Почему sealed class? ✅ Мы гарантируем, что UiState может быть только Loading, Success или Error → больше никто не сможет создать новый подкласс. 2️⃣ Когда enum class недостаточно (если у состояний есть данные)

Пример: Результат сетевого запроса

```kotlin
sealed class NetworkResult<out T> {
		data class Success<T>(val data: T) : NetworkResult<T>()
		data class Error(val errorMessage: String) : NetworkResult<Nothing>()
		object Loading : NetworkResult<Nothing>()
}
```

Почему не enum class? ❌ enum class может содержать только значения, но не хранить уникальные данные (например, Success(val data: T)). ✅ sealed class позволяет передавать параметры в дочерние классы. 3️⃣ Когда нужен when, который проверяет все случаи (exhaustive when)

Пример: Без else в when

```kotlin
fun handleState(state: UiState) = when (state) {
is UiState.Loading -> println("Загрузка...")
is UiState.Success -> println("Данные: ${state.data}")
is UiState.Error -> println("Ошибка: ${state.message}")
}
```

🔥 Фишка → если добавить новый подкласс в UiState, компилятор заставит обновить when. 4️⃣ Когда нужна альтернатива интерфейсам

Пример: Команды для UI

```kotlin
sealed class UiCommand {
	object ShowLoading : UiCommand()
	data class ShowMessage(val message: String) : UiCommand()
	object HideLoading : UiCommand()
}
```

Почему sealed class вместо interface? ✅ В sealed class можно объявлять объекты и данные в одном месте, а в interface этого нет.
[[kotlin]]