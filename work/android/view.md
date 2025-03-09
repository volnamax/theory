#### Что такое View в Android?

**View** объекты являются основными строительными блоками элементов пользовательского интерфейса (UI) в `Android`. View это простая прямоугольная коробка, которая реагирует на действия пользователя. Примеры включают `EditText`, `Button`, `CheckBox` и т. д. View относится к классу `android.view.View`, который является базовым классом всех классов UI.


 ### Почему view нельзя обновить не из ui потока?
    
В Android обновление пользовательского интерфейса (UI) не из основного потока (также известного как UI поток) запрещено из-за модели однопоточности, которую Android использует для управления пользовательским интерфейсом

###### ChatGPT said:

### **📌 Почему `View` нельзя обновить не из UI-потока?**

В Android **все операции, связанные с пользовательским интерфейсом (UI)**, должны выполняться в **главном (UI) потоке**. Это связано с **моделью однопоточности**, используемой в Android.

---

### **🔹 Причины запрета обновления UI из фонового потока**

1️⃣ **Android View Hierarchy не потокобезопасна**

- Все `View` элементы (TextView, Button, RecyclerView и т. д.) **хранят свое состояние в одном потоке**.
- Если несколько потоков одновременно изменяют UI, это может привести к **крашу** (`CalledFromWrongThreadException`) или **непредсказуемому поведению**.

2️⃣ **Один главный (UI) поток для рендеринга**

- **UI-рендеринг выполняется в главном потоке**, поэтому обновление `View` из других потоков не синхронизировано.
- Если поток обновит `View`, пока главный поток выполняет рендеринг, **возможно повреждение данных или пропуск обновления**.

3️⃣ **Избежание состояний гонки (`Race Condition`)**

- Если два потока одновременно обновляют `TextView`, одно из обновлений может **перезаписать другое**.
- Это делает UI **непредсказуемым и нестабильным**.

🔹 Пример ошибки

Попытка обновить UI из фонового потока приведет к ошибке:
```kotlin
Thread {
    textView.text = "Привет, мир!" // Ошибка! View нельзя обновить из фонового потока
}.start()

```

❌ Ошибка:
android.view.ViewRootImpl$CalledFromWrongThreadException: Only the original thread that created a view hierarchy can touch its views.
🔹 Как правильно обновлять UI из фонового потока?

🔹 Используем runOnUiThread (Activity)
```kotlin
Thread {
    val result = loadDataFromNetwork()
    runOnUiThread {
        textView.text = result  // ✅ Обновление UI в главном потоке
    }
}.start()
```
🔹 Используем Kotlin Coroutines + Dispatchers.Main (Современный подход)
```kotlin
lifecycleScope.launch(Dispatchers.IO) {
    val result = loadDataFromNetwork()
    withContext(Dispatchers.Main) {
        textView.text = result  // ✅ Обновление в UI-потоке
    }
}
```
ChatGPT said:
Жизненный цикл View и процесс отрисовки в Android

В Android каждый View проходит три основных этапа отрисовки:
📌 1. Measure (Измерение)

На этом этапе система измеряет размеры каждого View.

    Родительский View вызывает у дочернего метод measure(widthMeasureSpec, heightMeasureSpec),
    спрашивая, сколько места ему нужно.
    Дочерний View в ответ самостоятельно вычисляет и устанавливает свои размеры через setMeasuredDimension(width, height).
    Этот процесс идет рекурсивно сверху вниз, пока не будут измерены все View в иерархии.

✅ Пример кода:

override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    val width = MeasureSpec.getSize(widthMeasureSpec) // Получаем доступное пространство
    val height = MeasureSpec.getSize(heightMeasureSpec) 
    setMeasuredDimension(width, height) // Устанавливаем измеренные размеры
}

📌 2. Layout (Размещение)

Когда все View измерены, система начинает размещение (layout).

    Родитель вызывает метод layout(left, top, right, bottom),
    определяя, где именно разместить дочерний View на экране.
    Дочерние View используют эти координаты, чтобы позиционировать свои элементы.

✅ Пример:

override fun onLayout(changed: Boolean, left: Int, top: Int, right: Int, bottom: Int) {
    childView.layout(left, top, left + childWidth, top + childHeight)
}

    layout() задает координаты View внутри родителя.

📌 3. Draw (Отрисовка)

После измерения и размещения начинается этап отрисовки (draw()).

    View отрисовывает себя через onDraw(Canvas),
    используя Canvas для рисования графики (линий, текста, изображений и т. д.).
    Если ViewGroup, он вызывает dispatchDraw(Canvas), чтобы отрисовать дочерние View.

✅ Пример:

override fun onDraw(canvas: Canvas) {
    canvas.drawRect(0f, 0f, width.toFloat(), height.toFloat(), paint)
}
#### Жизненный цикл View:
## **Основные этапы жизненного цикла View**

| **Метод**                  | **Описание**                                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------------------------- |
| **onAttachedToWindow()**   | `View` был **добавлен** в окно. Можно запускать анимации, слушатели событий и инициализацию.         |
| **measure()**              | `View` **измеряется** перед отображением. Вызывает `onMeasure()`.                                    |
| **onMeasure()**            | Определяет **размер** `View`. Дочерний элемент должен вызвать `setMeasuredDimension(width, height)`. |
| **layout()**               | Определяет **размещение** `View` на экране. Вызывает `onLayout()`.                                   |
| **onLayout()**             | `ViewGroup` располагает **свои дочерние элементы** в нужных координатах.                             |
| **dispatchDraw()**         | `ViewGroup` рисует **все дочерние View**.                                                            |
| **draw()**                 | Управляет процессом **отрисовки** `View`. Вызывает `onDraw()`.                                       |
| **onDraw()**               | Основной метод **рисования содержимого View** (графика, текст, фигуры).                              |
| **onDetachedFromWindow()** | `View` **удален** из окна, все ресурсы можно очистить.                                               |
|                            |                                                                                                      |
1. **onAttachedToWindow()**: Этот метод вызывается, когда View было прикреплено к окну. Это первый шаг, когда View становится видимым в приложении. Здесь можно инициализировать анимации или начать мониторить изменения в системе. Это также сигнализирует, что View теперь может взаимодействовать с пользователем.
    
2. **measure()**: Это внутренний метод системы Android, который запускает процесс измерения View путём вызова onMeasure(). Этот метод не предназначен для переопределения, но он важен, так как инициирует определение размера View.
    
3. **onMeasure(int, int)**: В onMeasure(), View определяет, какого размера оно должно быть, исходя из предложений (specifications) родительского элемента. View должно вызвать setMeasuredDimension(int width, int height) для сохранения измеренных размеров. Это переопределяемый метод, который вы можете использовать, чтобы управлять размером вашего View.
    
4. **layout()**: Это другой внутренний метод системы, который вызывается после завершения процесса измерения. Он отвечает за расположение View в родительском контейнере. Этот метод также не предназначен для переопределения.
    
5. **onLayout(boolean, int, int, int, int)**: После метода layout(), onLayout() вызывается для того, чтобы View расположило себя и все свои дочерние элементы. В случае с ViewGroup, здесь определяется местоположение дочерних View. Это переопределяемый метод, где вы устанавливаете, где должны находиться дочерние элементы внутри ViewGroup.
    
6. **dispatchDraw(Canvas)**: dispatchDraw используется в ViewGroup для рисования дочерних View. Этот метод вызывается системой для рисования содержимого, и обычно его не нужно переопределять, но вы можете это сделать, чтобы управлять тем, как ваши дочерние View рисуются.
    
7. **draw(Canvas)**: Это внутренний метод, который управляет всем процессом рисования, включая вызов onDraw(), рисование фона, дочерних представлений, скроллирования и т. д. Этот метод обычно не переопределяется, так как он обрабатывает несколько задач рисования и делегирует основную работу onDraw().
    
8. **onDraw(Canvas)**: onDraw() — это где вы определяете, как View должно отображать свое содержимое. Это может включать рисование форм, текста, изображений или других графических элементов. Этот метод переопределяется, когда вы создаете пользовательский View и хотите контролировать его визуальное представление.

![[Pasted image 20250308153019.png]]
#### В чем разница между методами invalidateLayout и requestLayout?

- `invalidate()`: Вызывается, когда view должно быть перерисовано, но его размер и позиция не изменяются.
- `requestLayout()`: Вызывается, когда view должно быть измерено и размещено заново, что может привести к изменению размеров и позиции как этого view, так и его потомков.

#### Разница между `View.GONE` и `View.INVISIBLE`?

**View.GONE:**

* Что делает: Делает View невидимым и не занимает место в макете.
* Когда использовать: Используется, когда элемент должен исчезнуть из пользовательского интерфейса и не должен занимать пространство, как если бы его не было на экране.
    Пример: myView.visibility = View.GONE  // View исчезает и не занимает место

View.INVISIBLE:
* Что делает: Делает View невидимым, но оно все еще занимает место в макете.
* Когда использовать: Используется, когда элемент должен быть невидимым, но по-прежнему занимать место в макете. Это полезно, если нужно скрыть элемент, но сохранить его пространство для других элементов.
    Пример: myView.visibility = View.INVISIBLE  // View невидим, но занимает мест

#### Можете ли вы создать пользовательский View? Как?

Да, в Android можно создать **кастомный View** (Custom View). Это делается путем наследования от класса `View` или одного из его подклассов (`TextView`, `Button`, `ImageView`, `LinearLayout` и т. д.) и переопределения ключевых методов.
```kotlin 
import android.content.Context
import android.graphics.Canvas
import android.graphics.Color
import android.graphics.Paint
import android.util.AttributeSet
import android.view.View

class CustomCircleView @JvmOverloads constructor(
    context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {

    private val paint = Paint().apply {
        color = Color.RED
        style = Paint.Style.FILL
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        val radius = width.coerceAtMost(height) / 2f
        canvas.drawCircle(width / 2f, height / 2f, radius, paint)
    }
}

```
✔ **Когда стандартные View не подходят** (нужна сложная отрисовка).  
✔ **Когда нужно оптимизировать** (например, рисовать графику через `Canvas`).  
✔ **Когда нужно переиспользование UI-компонента**.


# ViewGroup VS View
### **Основные отличия `View` и `ViewGroup`**

| **Критерий**                      | **View**                          | **ViewGroup**                                     |
| --------------------------------- | --------------------------------- | ------------------------------------------------- |
| **Графическое представление**     | Да (видимый элемент UI)           | Нет (контейнер для `View`)                        |
| **Может содержать другие `View`** | ❌ Нет                             | ✅ Да                                              |
| **Примеры**                       | `Button`, `TextView`, `ImageView` | `LinearLayout`, `FrameLayout`, `ConstraintLayout` |

### **1. `View` – это элемент интерфейса**

`View` – это **базовый строительный блок** UI в Android.

- Он представляет **одиночный элемент**, который можно нарисовать на экране.
- `View` может реагировать на действия пользователя (например, `Button`, `TextView`, `ImageView`).
- Все UI-компоненты в Android наследуются от `View`.
```kotlin
@Composable
fun MyScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // View: Текст
        Text(text = "Привет, мир!", fontSize = 24.sp)

        Spacer(modifier = Modifier.height(16.dp)) // Отступ между элементами

        // View: Кнопка
        Button(onClick = { /* Обработчик клика */ }) {
            Text(text = "Нажми меня")
        }
    }
}

```

### **2. `ViewGroup` – это контейнер для `View`**

`ViewGroup` – это **невидимый контейнер**, который может содержать другие `View` (и даже `ViewGroup`).

- Он **не имеет графического представления**, но управляет дочерними `View`.
- Используется для **компоновки элементов** на экране.
- `ViewGroup` сам является `View`, но **он не рисует ничего сам**, а лишь управляет дочерними `View`.

#### Что такое Canvas?

**Canvas** — это одна из основных функций встроенного в Android фреймворка графического интерфейса, которая позволяет создавать разнообразные пользовательские интерфейсы и реализовывать сложную графику. С помощью этого инструмента разработчики могут реализовывать различные возможности, такие как рисование, анимация и обработка событий.
`Canvas` – это **поверхность для рисования**, которая позволяет разрабатывать кастомные элементы UI, такие как **графики, диаграммы, анимации и визуальные эффекты**.

```kotlin 
@Composable
fun CanvasExample() {
    Canvas(modifier = Modifier.fillMaxSize()) {
        drawCircle(
            color = Color.Blue,       // Цвет круга
            radius = 100f,            // Радиус круга
            center = Offset(size.width / 2, size.height / 2)  // Центр экрана
        )
    }
}

```
#### Что такое SurfaceView?

**SurfaceView** — это подкласс View, который предоставляет отдельный слой, на котором можно рисовать, не блокируя основной поток пользовательского интерфейса. Это полезно для рендеринга анимаций или видео, где требуется высокая производительность и меньшее взаимодействие с основным потоком UI.

```kotlin 
import android.content.Context
import android.graphics.Canvas
import android.graphics.Color
import android.graphics.Paint
import android.view.SurfaceHolder
import android.view.SurfaceView
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.viewinterop.AndroidView
import kotlinx.coroutines.delay
import kotlin.math.cos
import kotlin.math.sin

@Composable
fun SurfaceViewExample() {
    val context = LocalContext.current

    AndroidView(
        modifier = Modifier.fillMaxSize(),
        factory = { CustomSurfaceView(context) }
    )
}

class CustomSurfaceView(context: Context) : SurfaceView(context), SurfaceHolder.Callback {

    private val drawThread = DrawThread(holder)

    init {
        holder.addCallback(this)
    }

    override fun surfaceCreated(holder: SurfaceHolder) {
        drawThread.running = true
        drawThread.start()
    }

    override fun surfaceChanged(holder: SurfaceHolder, format: Int, width: Int, height: Int) {}

    override fun surfaceDestroyed(holder: SurfaceHolder) {
        drawThread.running = false
        drawThread.join()
    }
}

class DrawThread(private val surfaceHolder: SurfaceHolder) : Thread() {
    var running = false
    private val paint = Paint().apply {
        color = Color.RED
        style = Paint.Style.FILL
    }

    override fun run() {
        var angle = 0.0
        while (running) {
            val canvas: Canvas? = surfaceHolder.lockCanvas()
            if (canvas != null) {
                try {
                    canvas.drawColor(Color.BLACK)
                    val x = (300 + 100 * cos(angle)).toFloat()
                    val y = (500 + 100 * sin(angle)).toFloat()
                    canvas.drawCircle(x, y, 50f, paint)
                    angle += 0.1
                } finally {
                    surfaceHolder.unlockCanvasAndPost(canvas)
                }
            }
            sleep(16) // 60 FPS
        }
    }
}

```

#### Relative Layout против Linear Layout.

- **Relative Layout** позволяет размещать виджеты относительно друг друга или относительно родительского контейнера.
- **Linear Layout** размещает виджеты в линейной последовательности, горизонтально или вертикально.
### ✅ **Что выбрать в Jetpack Compose?**

| **Задача**                                     | **Лучший вариант в Compose** |
| ---------------------------------------------- | ---------------------------- |
| Размещать элементы **линейно**                 | `Column`, `Row`              |
| Размещать элементы **относительно друг друга** | `Box` + `align()`            |
#### Расскажите о Constraint Layout

**Constraint Layout** — это гибкий и мощный макет в Android, который позволяет создавать сложные пользовательские интерфейсы без вложенных макетов. Он предоставляет широкие возможности для определения ограничений для позиционирования и размера виджетов, что облегчает создание адаптивных интерфейсов.
**in compouse** 
```kotlin
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.Button
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.constraintlayout.compose.ConstraintLayout

@Composable
fun ConstraintLayoutExample() {
    ConstraintLayout(
        modifier = Modifier.fillMaxSize()
    ) {
        // Определяем ссылки (constraints)
        val (button, text) = createRefs()

        Text(
            text = "Привет, ConstraintLayout!",
            modifier = Modifier.constrainAs(text) {
                top.linkTo(parent.top, margin = 32.dp)
                start.linkTo(parent.start)
                end.linkTo(parent.end)
            }
        )

        Button(
            onClick = {},
            modifier = Modifier.constrainAs(button) {
                top.linkTo(text.bottom, margin = 16.dp)
                start.linkTo(parent.start)
                end.linkTo(parent.end)
            }
        ) {
            Text("Нажми меня")
        }
    }
}

```
#### Знаете ли вы, что такое дерево View? Как можно оптимизировать его глубину? (ViewTreeObserver)


**Дерево из View** — это иерархическая структура, которая представляет все виджеты (views) и макеты (layouts), составляющие пользовательский интерфейс в Android приложении. Оптимизировать глубину дерева View можно путем уменьшения количества вложенных макетов, использования **ConstraintLayout** для упрощения иерархии и избегания лишних макетов, которые не вносят вклад в пользовательский интерфейс. **ViewTreeObserver** может использоваться для прослушивания различных событий в дереве View, таких, как момент перед отрисовкой, что может быть полезно для дополнительных оптимизаций.

В **Jetpack Compose** нет традиционного **дерева View**, но концепция иерархии элементов остается важной.

## 📌 **Что такое дерево View?**

В **классическом Android (View)** каждый UI-элемент (`View`) является узлом **дерева View**, а `ViewGroup` (например, `LinearLayout`, `ConstraintLayout`) объединяет дочерние `View`.

В **Jetpack Compose** вместо `ViewTree` используется **Composable-иерархия**, состоящая из `Composable`-функций.

[[android]]