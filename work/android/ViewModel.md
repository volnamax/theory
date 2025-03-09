то такое ViewModel и в чем его польза?

ViewModel – это компонент Android Architecture Components, предназначенный для хранения и управления UI-данными в рамках жизненного цикла Activity или Fragment.
📌 Зачем нужен ViewModel?

🔹 Сохраняет данные при повороте экрана
🔹 Изолирует логику от UI (разделение UI и бизнес-логики)
🔹 Обеспечивает повторное использование данных
🔹 Поддерживает асинхронную обработку данных (LiveData, Flow)
🔹 Как работает ViewModel?

🔹 ViewModel создается при первом вызове Activity или Fragment
🔹 Переживает смену конфигурации (onDestroy() -> onCreate())
==🔹 Уничтожается только при уничтожении Activity==
🚀 Пример использования ViewModel
```kotlin
class MyViewModel : ViewModel() {
    val textData = MutableLiveData<String>()

    fun loadData() {
        textData.value = "Hello, ViewModel!"
    }
}

```
Связывание ViewModel с Activity:
```kotlin

class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: MyViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        viewModel = ViewModelProvider(this).get(MyViewModel::class.java)

        viewModel.textData.observe(this) { text ->
            textView.text = text
        }
    }
}
```



✅ Теперь при повороте экрана данные не теряются!
📌 Как ViewModel помогает избежать утечек памяти?

🔹 ViewModel не хранит ссылку на Activity (это предотвращает утечки памяти)
🔹 Данные не теряются при смене конфигурации
🔹 Уничтожается только при полном закрытии Activity или Fragment
🎯 Итоги

✔ ViewModel хранит данные между изменениями конфигурации
✔ Отделяет бизнес-логику от UI
✔ Поддерживает LiveData/Flow для реактивного обновления UI
✔ Помогает избежать утечек памяти
✔ Упрощает работу с асинхронными запросами (например, через Retrofit)

🔥 Вопрос на собеседовании:
📝 "Зачем нужен ViewModel?"
✅ Ответ: "ViewModel сохраняет данные при изменении конфигурации и помогает отделить логику от UI, предотвращая утечки памяти."

# Как поделиться ViewModel между фрагментами в Android?

Чтобы использовать одну ViewModel в нескольких Fragment, можно сделать ViewModel общей для Activity. Это обеспечит обмен данными между фрагментами без необходимости прямой связи между ними.
📌 Как это работает?

✔ ViewModel создается на уровне Activity
✔ Фрагменты получают один и тот же экземпляр ViewModel
✔ LiveData внутри ViewModel позволяет реактивно обновлять данные
🚀 Пример использования общей ViewModel
```kotlin
1. Создаем ViewModel

class SharedViewModel : ViewModel() {
    private val _text = MutableLiveData<String>() 
    val text: LiveData<String> = _text 

    fun updateText(newText: String) {
        _text.value = newText 
    }
}

2. Получаем ViewModel в Activity и передаем данные

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val viewModel = ViewModelProvider(this).get(SharedViewModel::class.java)

        // Устанавливаем начальное значение (например, при старте)
        viewModel.updateText("Привет из Activity!")
    }
}

3. Используем одну ViewModel в двух фрагментах
Первый фрагмент (отправляет данные)

class FirstFragment : Fragment() {
    private lateinit var viewModel: SharedViewModel

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_first, container, false)

        viewModel = ViewModelProvider(requireActivity()).get(SharedViewModel::class.java)

        view.findViewById<Button>(R.id.sendButton).setOnClickListener {
            viewModel.updateText("Данные из FirstFragment!")
        }

        return view
    }
}

Второй фрагмент (получает данные)

class SecondFragment : Fragment() {
    private lateinit var viewModel: SharedViewModel

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_second, container, false)

        viewModel = ViewModelProvider(requireActivity()).get(SharedViewModel::class.java)

        val textView = view.findViewById<TextView>(R.id.textView)

        // Подписываемся на изменения LiveData
        viewModel.text.observe(viewLifecycleOwner) { newText ->
            textView.text = newText
        }

        return view
    }
}

```
### **🔹 Как это работает?**

1️⃣ **ViewModel создается в `Activity`**  
2️⃣ **Оба фрагмента используют `requireActivity()`** для получения общей `ViewModel`  
3️⃣ **`FirstFragment` обновляет данные** → **`SecondFragment` автоматически получает их**

---

### **🎯 Итоги**

✔ Используем `ViewModelProvider(requireActivity())` для общей ViewModel  
✔ `LiveData` внутри `ViewModel` передает данные между фрагментами  
✔ Уменьшаем связь между фрагментами, повышая **чистоту архитектуры**  
✔ Работает даже при поворотах экрана, так как `ViewModel` привязана к `Activity`

🔥 **Вопрос на собеседовании:**  
📝 **"Как передавать данные между фрагментами без `Bundle` и `Interface`?"**  
✅ **Ответ:** _"Используем `ViewModel` на уровне `Activity`, подписываемся через `LiveData`."_

#### Как внутренне работает `ViewModel`?


**ViewModel** работает, сохраняя данные в специально отведенном хранилище, которое не уничтожается при изменении конфигурации, таких как поворот экрана. При изменении конфигурации активности или фрагмента, система автоматически пересоздает UI-компоненты, но **ViewModel** сохраняет свое состояние и данные, делая их немедленно доступными для нового экземпляра UI. Это достигается за счет хранения **ViewModel** в специализированном хранилище, которое привязано к **ViewModelStoreOwner** (например, активности или фрагменту), и не зависит от жизненного цикла отдельных компонентов UI.

[[android]]