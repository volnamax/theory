[[android]]

`WorkManager` — это библиотека для фоновых задач в Android, которая гарантированно выполняет работу, даже если приложение закрыто или устройство перезагружено. Разберёмся, как он работает под капотом.

---

### **1. Основные компоненты**

- `WorkRequest` – запрос на выполнение работы.
- `Worker` – класс, в котором выполняется код фоновой задачи.
- `WorkManager` – единая точка управления задачами.
- `WorkDatabase` – хранит данные о задачах, их статусах и зависимостях.
- `TaskExecutor` – управляет потоками выполнения.
- `Schedulers` – планировщики, определяющие, как и когда запускать задачи.

---

### **2. Как WorkManager обрабатывает задачи**

1. **Планирование работы**  
    Когда ты создаёшь `WorkRequest`, WorkManager сохраняет её в `Room`-базе (`WorkDatabase`).
    
2. **Определение способа выполнения**  
    WorkManager выбирает механизм выполнения в зависимости от API уровня:
    
    - **API 23+** → Использует `JobScheduler`.
    - **API 14–22** → Использует `AlarmManager + BroadcastReceiver`.
    - **API 21+** → Может использовать `JobScheduler`, если условия позволяют.
    - **API 24+** → Может использовать `Foreground Services`, если задача требует немедленного выполнения.
3. **Запуск задачи**
    
    - `TaskExecutor` создаёт `WorkerWrapper`, который достаёт задачу из базы.
    - `WorkerWrapper` вызывает метод `doWork()` у соответствующего `Worker`.
    - Если задача требует повторного выполнения (`PeriodicWork`), `WorkManager` пересоздаёт её с обновлённым временем старта.
4. **Обновление статуса**  
    WorkManager хранит состояние выполнения в `WorkDatabase` (`ENQUEUED`, `RUNNING`, `SUCCEEDED`, `FAILED`, `BLOCKED`, `CANCELLED`).
    

---

### **3. Что делает WorkManager надёжным?**

- **Хранение задач в БД** → даже если процесс убьют, задача не потеряется.
- **Резервные механизмы (JobScheduler / AlarmManager)** → на разных API.
- **Автоматическое повторение** → если задача не выполнилась, WorkManager повторит её.
- **Гибкая логика выполнения** → условия запуска (`Constraints`) позволяют задать, например, выполнение только при Wi-Fi.

---

### **4. Минусы WorkManager**

- Задержка при запуске задач из-за использования `JobScheduler`.
- Не подходит для задач, требующих моментального выполнения.
- Ограничения на `PeriodicWorkRequest` (минимальный интервал – 15 минут).

---

### **Пример кода**

```kotlin
class MyWorker(context: Context, params: WorkerParameters) : Worker(context, params) {
    override fun doWork(): Result {
        // Выполнение фоновой задачи
        return Result.success()
    }
}

// Запуск задачи
val workRequest = OneTimeWorkRequestBuilder<MyWorker>().build()
WorkManager.getInstance(context).enqueue(workRequest)
```

