#### Что такое `Fragment` и его жизненный цикл?

[inf](https://github.com/Senchick/android-interview?tab=readme-ov-file#что-такое-fragment-и-его-жизненный-цикл)

`Fragment` – это компонент приложения, который имеет свой жизненный цикл, получает события ввода и может быть добавлен в активность. Жизненный цикл фрагмента включает такие методы, как onAttach(), onCreate(), onCreateView(), onActivityCreated(), onStart(), onResume(), onPause(), onStop(), onDestroyView(), onDestroy(), onDetach():

- **onAttach()** - вызывается, когда фрагмент связывается с активностью.
- **onCreate()** - система вызывает этот метод, когда создает фрагмент. Создайте здесь компоненты, которые необходимы вне видимости пользователя.
- **onCreateView()** - вызывается для создания макета фрагмента. В этом методе вы должны создать и вернуть макет фрагмента.
- **onViewCreated()** - Вызывается сразу после onCreateView(), когда представление фрагмента уже создано. Это хорошее место для инициализации представлений.
- **onActivityCreated()** - вызывается, когда активность, к которой прикреплен фрагмент, завершила свой метод onCreate().
- **onStart()** - вызывается, когда фрагмент становится видимым.
- **onResume()** - вызывается, когда фрагмент получает фокус и может взаимодействовать с пользователем.
- **onPause()** - вызывается, когда пользователь теряет фокус на фрагменте, и можно сохранить изменения или обновления.
- **onStop()** - вызывается, когда фрагмент больше не видим.
- **onDestroyView()** - вызывается при удалении макета фрагмента.
- **onDestroy()** - вызывается при уничтожении фрагмента.
- **onDetach()** - вызывается, когда фрагмент отсоединяется от активности.
### **📌 Что такое Fragment в Android?**

**Fragment** — это **часть пользовательского интерфейса**, которая может быть встроена в `Activity`. Фрагменты:
✔️ Обладают своим жизненным циклом.  
✔️ Могут быть **повторно использованы** в разных активностях.  
✔️ Позволяют адаптировать **UI для разных экранов** (телефон/планшет).  
✔️ Используются для **динамического управления интерфейсом**.

Фрагменты работают **внутри активностей**, но могут **существовать независимо** от них.

---

### **🔹 Жизненный цикл Fragment**

Фрагмент проходит **несколько этапов жизненного цикла**, которые связаны с его отображением и уничтожением.

📌 **Основные методы:**

| **Метод**             | **Описание**                                                          |
| --------------------- | --------------------------------------------------------------------- |
| `onAttach()`          | Фрагмент **подключается** к активности.                               |
| `onCreate()`          | Создается фрагмент (здесь можно инициализировать данные).             |
| `onCreateView()`      | Создается **View** фрагмента (`return inflater.inflate(...)`).        |
| `onViewCreated()`     | Вызывается после `onCreateView()`, используется для инициализации UI. |
| `onActivityCreated()` | Активность завершила `onCreate()`, можно взаимодействовать с ней.     |
| `onStart()`           | Фрагмент становится **видимым**.                                      |
| `onResume()`          | Фрагмент **готов к взаимодействию**.                                  |
| `onPause()`           | Фрагмент теряет фокус, но еще видим.                                  |
| `onStop()`            | Фрагмент **перестает быть видимым**.                                  |
| `onDestroyView()`     | **Удаляется макет** (View) фрагмента.                                 |
| `onDestroy()`         | Фрагмент **уничтожается** (освободите ресурсы).                       |
| `onDetach()`          | Фрагмент **отключается от активности**.                               |

---

### **🔹 Жизненный цикл на схеме**

1️⃣ **Фрагмент создается и добавляется в активность**  
🔹 `onAttach()` → `onCreate()` → `onCreateView()` → `onViewCreated()` → `onActivityCreated()`  
2️⃣ **Фрагмент становится видимым**  
🔹 `onStart()` → `onResume()`  
3️⃣ **Фрагмент теряет фокус и скрывается**  
🔹 `onPause()` → `onStop()`  
4️⃣ **Фрагмент уничтожается**  
🔹 `onDestroyView()` → `onDestroy()` → `onDetach()`

---

### **🔹 Пример кода Fragment**

```kotlin 
class ExampleFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Раздуваем макет фрагмента
        return inflater.inflate(R.layout.fragment_example, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // Инициализация UI элементов
    }
}

```


## **3️⃣ Главные отличия `Activity` и `Fragment`**

|Функция|`Activity`|`Fragment`|
|---|---|---|
|**Независимость**|Самостоятельный экран|Работает только внутри `Activity`|
|**Жизненный цикл**|Управляется ОС (Android)|Управляется `Activity`|
|**Навигация**|Требуется `Intent` для перехода|Можно переключать без `Intent`|
|**Гибкость**|Неизменяемый экран|Можно заменять внутри `Activity`|
|**Применение**|Полноэкранные экраны|Части экрана (вкладки, списки)|

#### Почему рекомендуется использовать только конструктор по умолчанию для создания `Fragment`?


При восстановлении системой фрагментов (например, при пересоздании активности после изменения конфигурации) используется конструктор без параметров. Если вы создадите фрагмент с конструктором с параметрами и не сохраните эти параметры в onSaveInstanceState, вы можете потерять эти данные при пересоздании фрагмента, что приведет к ошибкам или непредвиденному поведению.


#### Что такое `Activity` и его жизненный цикл?

Activity - это компонент приложения, предоставляющий экран с пользовательским интерфейсом. Жизненный цикл Activity включает методы: onCreate(), onStart(), onResume(), onPause(), onStop(), onRestart(), onDestroy().

- **onCreate()** вызывается при первом создании активности. Здесь происходит инициализация.
- **onStart()** вызывается, когда активность становится видимой пользователю.
- **onResume()** вызывается, когда активность вступает в фокус и готова к взаимодействию с пользователем.
- **onPause()** вызывается при переходе активности в состояние "пауза", когда она все еще видима, но уже не в фокусе.
- **onStop()** вызывается, когда активность больше не видна пользователю.
- **onDestroy()** вызывается перед тем, как активность будет уничтожена.
- **onRestart()** вызывается после того, как активность была остановлена и снова была запущена пользователем.
==**onDestroy()** -- нет гарантии что система его вызовет !== 
**Activity** — это **основной компонент** Android-приложения, который представляет **один экран** с пользовательским интерфейсом.  
Каждое Android-приложение **начинается с Activity**, и оно управляет UI и взаимодействием пользователя.

---

## **🔹 Жизненный цикл Activity**

Жизненный цикл **Activity** управляется **системой Android** и проходит через несколько этапов.

| **Метод**     | **Описание**                                                                                       |
| ------------- | -------------------------------------------------------------------------------------------------- |
| `onCreate()`  | Активность создается. Здесь происходит **инициализация** UI, ViewModel, загрузка данных.           |
| `onStart()`   | Активность **становится видимой**, но еще не в фокусе.                                             |
| `onResume()`  | Активность **полностью активна** и взаимодействует с пользователем.                                |
| `onPause()`   | Активность **теряет фокус**, но **остается видимой** (например, поверх появилось диалоговое окно). |
| `onStop()`    | Активность больше **не видна пользователю** (например, он свернул приложение).                     |
| `onRestart()` | Активность возобновляется после `onStop()`.                                                        |
| `onDestroy()` | Активность **полностью уничтожена**, освобождаются ресурсы.                                        |

---

## **🔹 Жизненный цикл на схеме**

1️⃣ **Запуск активности**  
🔹 `onCreate()` → `onStart()` → `onResume()`  
2️⃣ **Переход в фон** (например, нажали "Домой")  
🔹 `onPause()` → `onStop()`  
3️⃣ **Возвращение активности**  
🔹 `onRestart()` → `onStart()` → `onResume()`  
4️⃣ **Закрытие активности**  
🔹 `onPause()` → `onStop()` → `onDestroy()`

```KOTLIN 
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Log.d("Lifecycle", "onCreate() вызван")
    }

    override fun onStart() {
        super.onStart()
        Log.d("Lifecycle", "onStart() вызван")
    }

    override fun onResume() {
        super.onResume()
        Log.d("Lifecycle", "onResume() вызван")
    }

    override fun onPause() {
        super.onPause()
        Log.d("Lifecycle", "onPause() вызван")
    }

    override fun onStop() {
        super.onStop()
        Log.d("Lifecycle", "onStop() вызван")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("Lifecycle", "onDestroy() вызван")
    }
}

```
✅ **Activity** — это **экран** в Android-приложении.  
✅ У нее есть **жизненный цикл**, который управляется системой.  
✅ Ключевые методы: `onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()`, `onDestroy()`, `onRestart()`.  
✅ **Важно управлять состоянием**: сохранять данные при `onPause()` и освобождать ресурсы в `onDestroy()`.

#### В чем разница между `onCreate()` и `onStart()`?

Основное различие между onCreate() и onStart() заключается в их роли в жизненном цикле Activity: onCreate() служит для инициализации Activity и вызывается один раз, когда Activity создается, в то время как onStart() делает Activity видимым для пользователя и может вызываться многократно каждый раз, когда Activity переходит в видимое состояние.

#### Когда вызывается только `onDestroy` для `Activity` без `onPause()` и `onStop()`?

Это может произойти в крайне редких случаях, например, если система уничтожает процесс приложения для освобождения ресурсов в условиях нехватки памяти. В обычной ситуации onPause() и onStop() всегда вызываются перед onDestroy().

#### Почему нам нужно вызывать `setContentView()` в `onCreate()` класса `Activity`?


setContentView() устанавливает макет пользовательского интерфейса для активности и должен быть вызван в onCreate(), чтобы до начала взаимодействия с пользователем активность имела загруженный и готовый интерфейс.
###### ChatGPT said:

### **📌 Почему нужно вызывать `setContentView()` в `onCreate()` класса `Activity`?**

Метод `setContentView(layoutId)` используется для **установки макета** пользовательского интерфейса (UI) активности.

---

## **🔹 Причины вызова `setContentView()` в `onCreate()`**

1. **Загрузка макета перед взаимодействием с пользователем**
    
    - `onCreate()` — **первый метод жизненного цикла**, он вызывается при создании `Activity`.
    - В этот момент **устанавливается UI**, чтобы пользователь мог видеть и взаимодействовать с ним.
    - Если **не вызвать `setContentView()`**, экран активности останется пустым.
2. **Подключение View элементов**
    
    - После `setContentView()` можно использовать `findViewById()` для работы с UI-элементами.
    - Без `setContentView()` попытка обратиться к View приведет к `NullPointerException`.
3. **Подготовка к дальнейшей настройке UI**
    
    - В `onCreate()` можно настроить кнопки, текстовые поля, анимации и другие элементы интерфейса.

---

## **🔹 Пример использования `setContentView()`**


```KOTLIN
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Устанавливаем макет активности
        setContentView(R.layout.activity_main)

        // Теперь можно работать с UI-элементами
        val button = findViewById<Button>(R.id.myButton)
        button.setOnClickListener {
            Toast.makeText(this, "Кнопка нажата!", Toast.LENGTH_SHORT).show()
        }
    }
}

```


## **🔹 Что будет, если не вызвать `setContentView()`?**

Если `setContentView()` не вызывается:

- `Activity` будет **пустым экраном** без UI.
- Вызов `findViewById()` приведет к **ошибке NullPointerException**.
- Приложение может **завершиться с ошибкой**.
✅ `setContentView()` **устанавливает UI** активности.  
✅ Вызывается **в `onCreate()`**, потому что это **первый метод жизненного цикла**.  
✅ Без него **UI не загрузится**, а работа с `findViewById()` приведет к ошибке.


#### Что нельзя делать в onCreate?

- Длительные операции, такие как сетевые запросы или обращение к базе данных, так как это может привести к "зависанию" приложения. Такие операции следует выполнять асинхронно.
- Изменение конфигурации, не связанной с инициализацией UI, которая может быть выполнена в других методах жизненного цикла.
#### Что такое `onSaveInstanceState()` и `onRestoreInstanceState()` в `Activity`?

- `onSaveInstanceState()` - Этот метод используется для сохранения данных перед паузой `Activity`.
- `onRestoreInstanceState()` - Этот метод используется для восстановления сохраненного состояния `Activity`, когда `Activity` пересоздается после уничтожения. Таким образом, `onRestoreInstanceState()` получает пакет, который содержит информацию о состоянии экземпляра.
#### Какие launch modes существуют для активити?

- **standard**: Запускает активити в новом экземпляре каждый раз.
- **singleTop**: Если экземпляр активити уже находится на вершине стека, он не создается заново, и вместо этого получает интент.
- **singleTask**: Создает новый задачный стек и запускает активити в новом экземпляре в этом стеке, но если активити уже существует в любом стеке, система очищает стек до этого активити и отправляет интент.
- **singleInstance**: Активити запускается в новом задачном стеке, но с гарантией того, что никакие другие активити не будут запущены в этом стеке.

В Android `launchMode` определяет, как новая `Activity` будет запускаться и управляться в стеке активностей (Back Stack). Всего есть **4 режима**, каждый из которых подходит для разных сценариев.

**1️⃣ standard (По умолчанию)**

📌 Каждый раз создается новый экземпляр Activity.

✅ Когда использовать:
✔ Когда нужно всегда открывать новую Activity.
✔ Например, открытие новых экранов профиля, страниц настроек и т. д.
Пример

```kotlin
<activity android:name=".MainActivity"
    android:launchMode="standard"/>
```

Как работает:

    Запускаем Activity A.
    Еще раз запускаем Activity A → создается новый экземпляр.

Стек активностей (Back Stack):

[A]
[A]
[A]

⛔ Минус: Могут создаваться ненужные дубликаты, что тратит память.
**2️⃣ singleTop (Один на вершине)**

📌 Если Activity уже находится на вершине стека, не создается новый экземпляр, а просто переиспользуется.

✅ Когда использовать:
✔ Если нажатие на иконку или кнопку "Домой" должно вернуть к текущей Activity, а не создавать новую.
✔ Подходит для экранов уведомлений, чатов, страниц деталей.
Пример

```kotlin
<activity android:name=".MainActivity"
    android:launchMode="singleTop"/>
```

Как работает:

    Запускаем Activity A.
    Запускаем Activity B.
    Еще раз запускаем Activity B.

Стек активностей:

[A]
[B]

❗ Activity B не создастся заново, а просто получит новый intent в onNewIntent().

⛔ Минус: Если Activity не на вершине, создается новый экземпляр.

**3️⃣ singleTask (Одна на весь стек)**

📌 Если Activity уже есть в стеке, система удаляет все Activity поверх неё и переиспользует существующую.

✅ Когда использовать:
✔ Подходит для главного экрана (MainActivity), чтобы не было дубликатов.
✔ Полезно для экранов, которые не должны дублироваться (например, главное меню, список задач и т. д.).
Пример
```kotlin 
<activity android:name=".MainActivity"
    android:launchMode="singleTask"/>
```
Как работает:

    Запускаем Activity A.
    Запускаем Activity B.
    Запускаем Activity C.
    Запускаем Activity A (снова).

Стек активностей:

[A]  ✅ `Activity B` и `Activity C` удалены из стека

🔹 Если Activity A уже есть в стеке, все Activity, которые были после нее (B и C), удаляются.
🔹 Activity A переиспользуется, новый intent передается в onNewIntent().
⛔ Минус: Может привести к неожиданному поведению, если пользователь ожидал, что Activity откроется заново.

**4️⃣ singleInstance (Одна на все приложение)**

📌 Гарантирует, что Activity будет существовать в отдельном стекe и всегда одна.

✅ Когда использовать:
✔ Если Activity не должна запускаться повторно и должна быть единственной, например, экран входа в систему или звонок.
✔ Полезно для экранов, которые не должны зависеть от других активностей.
Пример

```kotlin
<activity android:name=".CallActivity"
    android:launchMode="singleInstance"/>
```


Как работает:

    Запускаем Activity A.
    Запускаем Activity B.
    Запускаем Activity C.
    Запускаем CallActivity (звонок).

Стек активностей:

[A] [B] [C]  |  [CallActivity] (отдельный стек)

👉 CallActivity живет в отдельном стеке, а если кто-то попытается запустить ее еще раз — просто передаст intent в onNewIntent().

⛔ Минус: Если пользователь переключается обратно на другое Activity, singleInstance может сделать навигацию неудобной.

## **📌 Итог: Какой launch mode выбрать?**

| **Launch Mode**           | **Что делает**                                   | **Когда использовать**               |
| ------------------------- | ------------------------------------------------ | ------------------------------------ |
| `standard` (по умолчанию) | Всегда создается новая `Activity`.               | Обычные экраны (профиль, настройки). |
| `singleTop`               | Если `Activity` уже сверху, используется старая. | Чаты, экраны уведомлений.            |
| `singleTask`              | Удаляет все `Activity` сверху и переиспользует.  | Главное меню (`MainActivity`).       |
| `singleInstance`          | Запускает `Activity` в отдельном стеке.          | Видеозвонки, экран авторизации.      |

#### В чем разница между `Fragment` и `Activity`? Объясните взаимосвязь между ними.

Activity действует как контейнер для пользовательского интерфейса и управляет взаимодействием пользователя с экраном. Fragment, с другой стороны, представляет собой повторно используемую часть пользовательского интерфейса с собственным жизненным циклом, которая может быть встроена в активность. Фрагменты добавляют гибкость в проектирование интерфейса, позволяя активности включать несколько фрагментов на одном экране и переиспользовать фрагмент в разных активностях.

#### Когда следует использовать `Fragment` вместо `Activity`?

- Когда у вас есть некоторые компоненты пользовательского интерфейса, которые должны использоваться в различных `Activity`
- Когда несколько представлений могут быть отображены бок о бок, как например `ViewPager`

### **1️⃣ Повторное использование UI-компонентов**

Если **один и тот же UI-блок** нужен в нескольких `Activity`, лучше создать `Fragment` и переиспользовать его.  
📌 **Пример**: Форма авторизации, список товаров, карточка пользователя.
```kotlin
class LoginFragment : Fragment(R.layout.fragment_login)
class RegisterFragment : Fragment(R.layout.fragment_register)
```
```kotlin 
supportFragmentManager.beginTransaction()
    .replace(R.id.container, LoginFragment())
    .commit()

```

### **2️⃣ Многоэкранные приложения (ViewPager / Tabs)**

Если нужно **переключать экраны свайпом**, например, в `ViewPager` или `BottomNavigation`, используйте `Fragment`.  
📌 **Пример**: Вкладки в `WhatsApp` (Чаты, Статус, Звонки).
```kotlin 
val adapter = ViewPagerAdapter(supportFragmentManager)
	adapter.addFragment(HomeFragment(), "Главная")
	adapter.addFragment(SettingsFragment(), "Настройки")
	viewPager.adapter = adapter
```
### **4️⃣ Использование `Navigation Component`**

Google рекомендует использовать `Fragment` в связке с `Navigation Component`, так как это **упрощает навигацию**.
```kotlin 
findNavController().navigate(R.id.action_homeFragment_to_detailsFragment)
```

## **Когда лучше использовать `Activity` вместо `Fragment`?**

|**Сценарий**|**Лучший выбор**|
|---|---|
|Запуск независимого экрана|`Activity`|
|Разделение логики (разные entry points)|`Activity`|
|Переход между разными разделами приложения|`Activity`|
|Отображение вложенных UI-компонентов|`Fragment`|
|Адаптация интерфейса (планшеты, вкладки)|`Fragment`|

---
#### В чем разница между `FragmentPagerAdapter` и `FragmentStatePagerAdapter`?
- `FragmentPagerAdapter`: Каждый `Fragment`, посещенный пользователем, будет храниться в памяти, но представление будет уничтожено. Когда страница снова посещается, то создается представление, а не экземпляр фрагмента.
- `FragmentStatePagerAdapter`: Здесь экземпляр фрагмента будет уничтожен, когда он не виден пользователю, за исключением сохраненного состояния фрагмента.

## **1️⃣ `FragmentPagerAdapter`**

🔹 **Хранит все фрагменты в памяти**, но уничтожает их представление (`View`).  
🔹 Используется, если **у вас небольшое количество страниц (фрагментов)**.  
🔹 Повторно использует объекты `Fragment`, **не удаляя их из памяти**.

📌 **Пример использования**:
```kotlin 
class MyPagerAdapter(fm: FragmentManager) : FragmentPagerAdapter(fm, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT) {
    override fun getItem(position: Int): Fragment = when (position) {
        0 -> HomeFragment()
        1 -> ProfileFragment()
        else -> SettingsFragment()
    }
    override fun getCount(): Int = 3
}
```

📌 **Как работает?**

- `View` уничтожается при уходе за экран, но сам `Fragment` остаётся в памяти.
- Подходит для **статичных вкладок** (например, `BottomNavigationView` с 3–5 экранами).
- **Минус**: Использует больше памяти, потому что хранит все фрагменты.

## **2️⃣ `FragmentStatePagerAdapter`**

🔹 **Уничтожает фрагменты при уходе за экран**, оставляя только сохраненное состояние.  
🔹 Используется, если **у вас много страниц, например, лента или список товаров**.  
🔹 Экономит память, так как `Fragment` создается заново при возврате на страницу.

📌 **Пример использования**:
```kotlin 
class MyPagerAdapter(fm: FragmentManager) : FragmentStatePagerAdapter(fm, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT) {
    override fun getItem(position: Int): Fragment = PageFragment.newInstance(position)
    override fun getCount(): Int = 100 // Большое количество страниц
}
```

**Как работает?**

- `Fragment` полностью уничтожается, если уходит за экран.
- При возврате создаётся новый экземпляр `Fragment`, но состояние сохраняется.
- Подходит для **динамического контента** (`ViewPager` для фото, списков и новостей).
- **Минус**: Пересоздает `Fragment`, что может привести к небольшим задержкам.

## **Итог: что выбрать?**

|**Критерий**|**FragmentPagerAdapter**|**FragmentStatePagerAdapter**|
|---|---|---|
|Потребление памяти|Высокое (хранит все `Fragment`)|Низкое (удаляет `Fragment`)|
|Количество фрагментов|Небольшое (3-5)|Большое (10+)|
|Поведение `Fragment`|Сохраняется, но его `View` удаляется|Полностью удаляется|
|Подходит для|`BottomNavigation`, `TabLayout`|Динамические списки, галереи|

🚀 **Вывод**:

- `FragmentPagerAdapter` → **для статичных вкладок** (3-5 страниц).
- `FragmentStatePagerAdapter` → **для динамического контента** (лента, новости, фото).

#### В чем разница между добавлением/заменой фрагмента в backstack?

При добавлении фрагмента, он размещается поверх предыдущего, не удаляя его, в то время как замена удаляет текущий фрагмент и добавляет новый. Использование addToBackStack() позволяет возвращаться к предыдущему фрагменту с помощью кнопки "Назад".

### **Разница между добавлением (`add()`) и заменой (`replace()`) фрагмента в BackStack**

Когда мы управляем `FragmentTransaction`, есть два основных подхода: **добавление (`add()`)** и **замена (`replace()`)**.

---

### **1️⃣ `add()` – Добавление фрагмента**

🔹 **Новый фрагмент добавляется поверх текущего**, не удаляя его.  
🔹 Если нажать "Назад", **текущий фрагмент закроется, и отобразится предыдущий**.  
🔹 Подходит, если хотите **сохранить предыдущий фрагмент в памяти**.

📌 **Пример:**
```kotlin 
supportFragmentManager.beginTransaction()
    .add(R.id.container, NewFragment()) // Добавляем новый фрагмент поверх старого
    .addToBackStack(null) // Позволяет вернуться назад
    .commit()

```
📌 **Как работает?**

1. `FragmentA` -> `add(FragmentB)`
2. **Оба фрагмента существуют**, но `FragmentB` перекрывает `FragmentA`.
3. Если нажать "Назад", **`FragmentB` удаляется, и `FragmentA` снова видно**.

✔ **Когда использовать?**

- Если хотите **сохранить предыдущий фрагмент** (например, диалог или дополнительное окно).
- Когда у вас **сложные анимации**, и нужно плавное перекрытие UI.
### **2️⃣ `replace()` – Замена фрагмента**

🔹 **Удаляет старый фрагмент и добавляет новый**.  
🔹 Если нажать "Назад", **удаленный фрагмент не восстановится**.  
🔹 Экономит память, так как **старый фрагмент полностью уничтожается**.

📌 **Пример:**
```kotlin 
supportFragmentManager.beginTransaction()
    .replace(R.id.container, NewFragment()) // Удаляем старый фрагмент, добавляем новый
    .addToBackStack(null) // Позволяет вернуться назад
    .commit()
```

## **Основные отличия `add()` и `replace()`**

| **Критерий**              | **add()**                           | **replace()**                                |
| ------------------------- | ----------------------------------- | -------------------------------------------- |
| Старый фрагмент           | Сохраняется                         | Удаляется                                    |
| Потребление памяти        | Больше                              | Меньше                                       |
| Использование `BackStack` | Можно вернуться к старому фрагменту | Новый фрагмент создается заново при возврате |
| Подходит для              | Диалогов, дополнительных экранов    | Основных экранов, переключения вкладок       |
|                           |                                     |                                              |
|                           |                                     |                                              |
#### Как осуществляется коммуникация между двумя `Fragments`?
В Android Fragment не может напрямую взаимодействовать с другим Fragment. Вместо этого есть несколько способов наладить связь между Fragment-ами.
1️⃣ Через ViewModel (рекомендуемый способ ✅)

Используется общий ViewModel, который сохраняется в Activity и доступен обоим Fragment-ам.

    Плюсы:
    ✔ Не требует прямого доступа Fragment к другому Fragment.
    ✔ Учитывает жизненный цикл Fragment.
    ✔ Разделение ответственности (Separation of Concerns).

📌 Пример:

    Создаем SharedViewModel:
```kotlin
class SharedViewModel : ViewModel() {
    private val _message = MutableLiveData<String>()
    val message: LiveData<String> get() = _message

    fun sendMessage(msg: String) {
        _message.value = msg
    }
}

    Передаем данные из FragmentA:

class FragmentA : Fragment() {
    private lateinit var viewModel: SharedViewModel

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel = ViewModelProvider(requireActivity())[SharedViewModel::class.java]

        // Отправка данных
        button.setOnClickListener {
            viewModel.sendMessage("Привет от FragmentA!")
        }
    }
}
Получаем данные в FragmentB:

class FragmentB : Fragment() {
    private lateinit var viewModel: SharedViewModel

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel = ViewModelProvider(requireActivity())[SharedViewModel::class.java]

        // Наблюдение за изменениями
        viewModel.message.observe(viewLifecycleOwner) { message ->
            textView.text = message
        }
    }
}
```


⚡ Когда использовать?
✔ Когда Fragments должны обмениваться данными внутри одной Activity.
✔ Когда Fragments могут не существовать одновременно (например, ViewPager).
2️⃣ Через Activity (старый способ ❌)

Один Fragment может передавать данные Activity, а другой Fragment их получать через Activity.

📌 Пример:
```kotlin 
class MainActivity : AppCompatActivity(), FragmentA.FragmentAListener {
    override fun onMessageSent(message: String) {
        val fragmentB = supportFragmentManager.findFragmentById(R.id.fragmentB) as FragmentB
        fragmentB.updateText(message)
    }
}

📌 Передача данных из FragmentA в Activity:

class FragmentA : Fragment() {
    private var listener: FragmentAListener? = null

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is FragmentAListener) {
            listener = context
        }
    }

    fun sendMessage() {
        listener?.onMessageSent("Привет от FragmentA!")
    }
}

📌 Обновление FragmentB:

class FragmentB : Fragment() {
    fun updateText(message: String) {
        textView.text = message
    }
}
```


⚡ Когда использовать?
🚫 Этот способ устарел и не рекомендуется, так как Activity становится посредником и увеличивает связность кода.

**3️⃣ Через FragmentManager и setFragmentResult() (лучше, чем Activity, но хуже ViewModel)**

    Работает, даже если Fragment был уничтожен.
    Предназначен для передачи небольших данных между Fragment.

```kotlin

📌 Передача данных (FragmentA):

setFragmentResult("requestKey", bundleOf("message" to "Привет от FragmentA!"))

📌 Получение данных (FragmentB):

setFragmentResultListener("requestKey") { _, bundle ->
    val result = bundle.getString("message")
    textView.text = result
}

```

⚡ Когда использовать?
✔ Когда фрагменты находятся в одной Activity и не хотят зависеть от ViewModel.
✔ Когда данные передаются один раз (например, при закрытии DialogFragment).

#### Что такое retained `Fragment`?

По умолчанию, `Fragments` уничтожаются и пересоздаются вместе с их родительскими `Activities` при изменении конфигурации. Вызов `setRetainInstance(true)` позволяет нам обойти этот цикл уничтожения и пересоздания, сигнализируя системе о 
сохранении текущего экземпляра фрагмента, когда `Activity` пересоздается.

```kotlin

class RetainedFragment : Fragment() {
    var counter: Int = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        retainInstance = true  // Фрагмент будет сохранен при пересоздании Activity
    }
}

class MainActivity : AppCompatActivity() {
    private var retainedFragment: RetainedFragment? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        retainedFragment = supportFragmentManager.findFragmentByTag("RetainedFragment") as? RetainedFragment

        if (retainedFragment == null) {
            retainedFragment = RetainedFragment()
            supportFragmentManager.beginTransaction()
                .add(retainedFragment!!, "RetainedFragment")
                .commit()
        }

        // Используем retainedFragment для хранения данных
        retainedFragment?.counter = 10
    }
}
	 
```

### **Когда использовать Retained Fragment?**

✔ **Когда нужно сохранить данные между поворотами экрана**, например:

- Выполнение **фоновой загрузки** данных.
- Сохранение **состояния приложения**.
- Хранение **объекта ViewModel** (но лучше использовать `ViewModel`).  
    ✔ **Когда нужно сохранить тяжелые объекты**, чтобы избежать пересоздания.

---

### **Почему Retained Fragment устарел?**

📌 **Недостатки:**

- `setRetainInstance(true)` **больше не поддерживается в Fragment API**.
- Управление `Fragment` вручную может привести к утечкам памяти.
- Вместо него **лучше использовать `ViewModel`**, который автоматически сохраняется.

### **Вывод**

❌ `Retained Fragment` устарел.  
✅ Используйте `ViewModel` для сохранения данны


#### Какова цель `addToBackStack()` при выполнении транзакции фрагмента?

При вызове `addToBackStack()`, транзакция замены сохраняется в стеке возврата, так что пользователь может отменить транзакцию и вернуть предыдущий фрагмент, нажав кнопку Назад.

### **Как работает `addToBackStack()`?**

1. **Обычно**, когда мы заменяем фрагмент с помощью `replace()`, **старый фрагмент уничтожается**, и при нажатии "Назад" приложение просто закрывается.
2. **Но** если перед вызовом `commit()` добавить `addToBackStack(null)`, предыдущий фрагмент **не уничтожается**, а сохраняется в **стеке возврата**, и при нажатии "Назад" он снова становится видимым.

📌 **Пример кода без `addToBackStack()` (Фрагмент заменяется без возврата)**

```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.container, SecondFragment()) // Заменяет текущий фрагмент
    .commit()

```
При нажатии "Назад" **приложение закроется**, так как предыдущий фрагмент не сохранен.
📌 **Пример с `addToBackStack()` (Фрагмент сохраняется в истории)**
```kotlin 
supportFragmentManager.beginTransaction()
    .replace(R.id.container, SecondFragment()) // Заменяет текущий фрагмент
    .addToBackStack(null) // Сохраняет старый фрагмент в истории
    .commit()

```
🔹 Теперь при нажатии "Назад" **пользователь вернется к предыдущему фрагменту** вместо закрытия приложения.

---

### **Когда использовать `addToBackStack()`?**

✔ **Когда нужно сохранять навигацию между фрагментами**, например:

- Открытие нового экрана (например, детальной информации).
- Реализация многоэкранного интерфейса внутри одной `Activity`.

✔ **Когда пользователь должен иметь возможность вернуться назад**.

---

### **Когда НЕ использовать `addToBackStack()`?**

❌ **Когда текущий фрагмент должен быть заменен навсегда** (например, при смене главного экрана).  
❌ **Когда не нужен возврат к предыдущему фрагменту** (например, при открытии фрагмента из уведомления).

✔ **Когда пользователь должен иметь возможность вернуться назад**.

Как удалить последний фрагмент из стека?

Если нужно удалить последний фрагмент из BackStack, используйте:

supportFragmentManager.popBackStack()

📌 Удаление ВСЕХ фрагментов из BackStack

supportFragmentManager.popBackStack(null, FragmentManager.POP_BACK_STACK_INCLUSIVE)

### **Вывод**

✔ `addToBackStack()` **нужен для сохранения транзакции** и возврата к предыдущему фрагменту.  
✔ Без него **замененный фрагмент уничтожается**, и "Назад" закрывает приложение.  
✔ Используйте `popBackStack()`, чтобы управлять стеком фрагментов.

==передача дата класса через интент между двумя активити==
если мы передаем более 6мб переполнится бандл
-> сеарилизация так как не знаем как работать с не примитивами 
реализуется она с помощью serializeble parseleble 

что лучше использовать parseleble  vs serializeble 
parseleble  тк он не использует рефлексию в отличее от serializeble

[[android]]