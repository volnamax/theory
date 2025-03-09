[[Coroutine Scope]] отменяет все дочерние корутины при ошибке, supervisorScope — нет, другие корутины продолжают выполняться."

✅ Пример coroutineScope (отменит все корутины при ошибке):

```java
suspend fun test() = coroutineScope {
	launch { throw RuntimeException("Ошибка!") }
	launch { println("Этот код не выполнится") }
}
```

✅ Пример supervisorScope (не отменит другие корутины при ошибке):

```java
suspend fun test() = supervisorScope {
	launch { throw RuntimeException("Ошибка!") }
	launch { println("Этот код выполнится") } // Выполнится!
}
```

👉 supervisorScope полезен, когда нужно продолжить выполнение других задач, даже если одна из них упала. Это полезно в ситуациях, когда необходимо обеспечить независимое выполнение дочерних корутин внутри одной области видимости.

[[kotlin]] 