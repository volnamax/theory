💬 Ответ: "GlobalScope живет, пока работает приложение (может вызвать утечки памяти), viewModelScope уничтожается с ViewModel, lifecycleScope привязан к Activity/Fragment."

✅ Пример:

```java
// Внутри ViewModel
fun fetchData() = viewModelScope.launch {
		val data = repository.getData()
}
```

👉 На Android не используй GlobalScope! Используй viewModelScope или lifecycleScope.

- **lifecycleScope**: Привязан к жизненному циклу **Activity** или **Fragment** и автоматически отменяет корутину, когда компонент уничтожается.
- **viewModelScope**: Привязан к жизненному циклу **ViewModel** и отменяет корутины при очистке **ViewModel**.
- **GlobalScope**: Глобальный скоуп корутины, который не привязан к жизненному циклу и должен использоваться **с осторожностью**, так как может привести к утечкам памяти.

[[kotlin]]
[[supervisorScope]]
