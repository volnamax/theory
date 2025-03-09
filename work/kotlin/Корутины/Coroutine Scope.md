### Что такое `Coroutine Scope`?

обертка над [[Coroutine Context]]. в чем отличие?
> - *контекст* является набором параметров для выполнения корутины, которые есть обязательно у любой из корутин.
> - *скоуп* предназначен для объединения всех запущенных корутин внутри него, а также под капотом передает *Job*, которая будет их объединять и будет родительской для всех запущенных корутин в скоупе.

CoroutineScope – это область, в которой выполняются корутины. Он определяет жизненный цикл корутин и управляет их отменой, когда больше не нужно их выполнять.

1. Почему нужен CoroutineScope?

Без CoroutineScope, если корутина запущена, то:

```
Она продолжит выполняться даже если объект (например, ViewModel или Activity) уничтожится, что может привести к утечкам памяти.
Ее сложно отменить без CoroutineScope.

```

💡 CoroutineScope управляет корутинами и автоматически отменяет их, когда больше не нужно выполнять задачи. 

**Как создается CoroutineScope?** 
	✅ 1. Глобальный (GlobalScope)

Запускает корутину, которая живет столько же, сколько процесс приложения.

```kotlin
GlobalScope.launch {
	delay(1000)
	println("Работает даже после закрытия экрана!")
}
```

❌ Минус: Не отменяется автоматически, может вызвать утечки памяти.
	✅ 2. Локальный (вручную создается)

Если нам нужно управлять корутинами вручную:

```kotlin
val myScope = CoroutineScope([Dispatchers.IO](<http://dispatchers.io/>))
myScope.launch {
	delay(1000)
	println("Я выполняюсь в IO потоке!")
}
```

❌ Минус: Нужно вручную отменять с помощью myScope.cancel().

✅ 3. Живущий вместе с объектом (Например, в ViewModel)
Используется в ViewModel, Activity, Fragment, чтобы не было утечек памяти.

```kotlin
class MyViewModel : ViewModel() {
private val viewModelScope = CoroutineScope(Dispatchers.Main + SupervisorJob())
fun fetchData() {
    viewModelScope.launch {
        val data = getDataFromNetwork()
        println(data)
    }
}

override fun onCleared() {
    super.onCleared()
    viewModelScope.cancel() // Останавливаем корутины при уничтожении ViewModel
}

```

✔ Плюс: Корутину автоматически отменяет, когда ViewModel уничтожается. 

✅ 4. Встроенный в ViewModelScope
В AndroidX ViewModel лучше использовать viewModelScope:

```kotlin
class MyViewModel : ViewModel() {
	fun fetchData() {
		viewModelScope.launch {
			val data = getDataFromNetwork()
			println(data)
		}
	}
}
```

✔ Плюс: Ничего не нужно отменять вручную.

✅ 5. Внутри LifecycleScope (Activity, Fragment)

Для Activity или Fragment корутины привязываются к их жизненному циклу:

```kotlin
class MyActivity : AppCompatActivity() {
override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
   lifecycleScope.launch {
        val data = getDataFromNetwork()
        println(data)
    }
}

```

}

✔ Плюс: Автоматически отменяет корутины при уничтожении экрана. 3.

Как ответить на собеседовании?

**coroutineScope**. Это функция-билдер, которая создает новую область видимости корутины. Все корутины, запущенные внутри этой области, должны быть завершены, прежде чем coroutineScope вернет управление вызывающей стороне. Это полезно для структурирования кода и управления жизненным циклом группы корутин.

**"CoroutineScope в Kotlin – это область, в которой выполняются корутины. Он управляет их жизненным циклом, позволяя автоматически отменять ненужные задачи. В Android принято использовать viewModelScope в ViewModel и lifecycleScope в Activity/Fragment, чтобы корутины завершались вместе с объектами и не вызывали утечек памяти."**

[[kotlin]] 