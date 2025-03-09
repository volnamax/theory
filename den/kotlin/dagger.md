### Основные компоненты Dagger:

1. **@Module и @Provides**
    
    - `@Module` указывает, что класс предоставляет зависимости.
    - `@Provides` используется для определения метода, который создает и предоставляет зависимость.
    
```kotlin  
@Module 
class NetworkModule {     
	@Provides     
	fun provideRetrofit(): Retrofit {
	         return Retrofit.Builder()
			         .baseUrl("https://api.example.com")
			         .build()
	} 
}
```
    
2. **@Inject**
    
    - Используется для автоматического внедрения зависимостей в конструкторы или поля.
    
```kotlin
class ApiService @Inject constructor(private val retrofit: Retrofit) {
	fun fetchData() { /*...*/ } 
}
```
    
3. **@Component**
    
    - Определяет связи между зависимостями и их потребителями.
    - Dagger генерирует код для компонента во время компиляции.
    
```kotlin
@Component(modules = [NetworkModule::class]) 
interface AppComponent {     
	fun inject(activity: MainActivity) 
}   
```
    
4. **@Singleton и Scope’ы**
    
    - `@Singleton` обеспечивает создание единственного экземпляра объекта.
    - Для кастомных Scope’ов создаются аннотации.
    
```kotlin
@Singleton 
@Component(modules = [NetworkModule::class]) 
interface AppComponent {     
	fun inject(activity: MainActivity) 
}
```
    
5. **Subcomponents**
    
    - Подкомпоненты позволяют делить граф зависимостей на уровни.
    - Полезны для работы с ViewModel и фичами.
    
```kotlin
@Subcomponent 
interface ActivityComponent {     
	fun inject(activity: MainActivity) 
}
```


### **`@Provides`**

✅ Используется, когда нужно создать объект вручную.  
✅ Поддерживает сложную логику и кастомные фабрики.  
✅ Может возвращать конкретные экземпляры классов.

**Пример:**

```kotlin
@Module 
class NetworkModule {     
	@Provides     
	fun provideRetrofit(): Retrofit {         
		return Retrofit.Builder()
				.baseUrl("https://api.example.com")
				.build()     
	} 
}
```

---

### **`@Binds`**

✅ Используется только для связывания интерфейсов с их реализациями.  
✅ Работает быстрее, так как не требует дополнительного создания объекта.  
✅ Метод должен быть `abstract`, а модуль — `@Module` и `abstract`.

**Пример:**

```kotlin
interface ApiService {     
	fun fetchData() 
}  

class ApiServiceImpl @Inject constructor() : ApiService {     
	override fun fetchData() { /* Реализация */ } 
}  

@Module abstract class ServiceModule {     
	@Binds     
	abstract fun bindApiService(impl: ApiServiceImpl): ApiService 
}
```
