[[kotlin]] [[coroutines]] [[android]]

```kotlin
try {
	doWork()
} catch (e: Exception) {
	// do something
}

suspend fun doWork()
```

*launch* и *async* обрабатываются по-разному

```kotlin
launch {
	try {
		doWork()
	} catch (e: Exception) {
		// do something
	}
}
```

```kotlin
val deferred: Deferred<Data> = async {
	doWork()
}

try {
	deferred.await()
} catch (e: Exception) {
	// do something
}
```

можно подписаться на уведомления корутины, в которую придет объект *Throwable?* (не вызовется в случае *CancellationException*, нельзя обработать исключения)

```kotlin
job.invokeOnCompletion { cause: Throwable? ->
	if (cause != null) {
		// something gone wrong
	} else {
		// everything is fine
	}
}
```

отмена - это тоже ошибка. отмена джоб не приведет к отмене ее родителя, но дочерние корутины будут остановлены.

можно определить поведение для всех необработанных исключений, которые происходят в текущем контексте выполнения корутин:

```kotlin
CoroutineExceptionHandler { context, error: Throwable ->
	logError(error)
}

CoroutineScope(exceptionHandler)

scope.launch(exceptionHandler)
```

можно перехватывать исключения (кроме *CancellationException*), но повлиять на выполнение программы уже не получится. *CoroutineExceptionHandler* вызывается в последнюю очередь после того как произошла ошибка (может вызваться на любом потоке). обычно используется для логирования ошибок и отправления их в аналитику.

в случае если выполнение корутины отменяется с ошибкой или через вызов *cancel()*, вызвать в ней *suspend* функцию не получится, а нам надо что-то сделать, например завершить поток. для таких случаев есть *NonCancellable Job* (использовать только в *withContext*, иначе нарушаются принципы [[structured concurrency]]).

```kotlin
val inputStream: InputStream
try {
	doWork(inputStream)
} catch (e: Exception) {
	// do something
} finally {
	withContext(NonCancellable) {
		shutdown(inputStream)
	}
}
```