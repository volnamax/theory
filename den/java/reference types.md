[[java]] 

## Strong reference

при создании объекта по дефолту задается стронг ссылка. гк не убьет объект пока на него указывает стронг сслыка (убьет, когда ссылка будет указывать на null).
```java
MyClass obj = new MyClass();
```
## Weak reference

гк убьет объект с вик ссылкой, как только найдет ссылку
```java
WeakReference<MyClass> obj = new MyClass();
```

## Soft reference

гк убьет объект только тогда, когда использование памяти в куче будет зашкаливать (близко к порогу)

```java
SoftReference<MyClass> obj = new MyClass();
```

## Phatom reference

самый слабый тип. гк убьет объект с такой ссылкой в любой момент времени. прежде чем убить, гк вызывает метод *finalize()* и JVM кладет их в очередь ‘reference queue’

```java
PhantomReference<MyClass> obj = new MyClass();
```



