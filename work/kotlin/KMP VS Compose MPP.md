### **Разница между Kotlin Multiplatform (KMP) и Jetpack Compose Multiplatform (Compose MPP)**

Обе технологии используются для создания **мультиплатформенных** приложений, но они решают разные задачи:

| **Характеристика**      | **Kotlin Multiplatform (KMP)**                                                      | **Jetpack Compose Multiplatform (Compose MPP)**                 |
| ----------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Что это?**            | Технология для написания **общего (shared) кода** для разных платформ               | Фреймворк для создания **мультиплатформенного UI**              |
| **Цель**                | Общий код для логики (бизнес-логика, работа с API, БД)                              | Общий код для UI                                                |
| **Работает на**         | Android, iOS, Desktop (JVM, macOS, Windows, Linux), Web                             | Android, Desktop (Windows, macOS, Linux), Web, iOS (в процессе) |
| **Ключевые технологии** | `expect/actual`, Ktor (сеть), SQLDelight (БД), coroutines                           | Compose, `@Composable`, Skia (рендеринг)                        |
| **Архитектура**         | Разделение `commonMain`, `androidMain`, `iosMain`                                   | UI пишется один раз, работает на всех платформах                |
| **Как работает?**       | Код разделяется по платформам (`commonMain`, `androidMain`, `iosMain`)              | UI описан в одном коде, адаптируется под платформу              |
| **Пример**              | `expect fun getPlatformName(): String` → `actual fun getPlatformName() = "Android"` | `Button(onClick = { }) { Text("Click Me") }`                    |
[[kotlin]]