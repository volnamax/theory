[[java]]

## 1. Marking

гц маркирует объекты без ссылок([[reference types]])
![[Pasted image 20231202223253.png]]

## 2. Deletion

гц удаляет объекты без ссылок. при этом memory allocator хранит список ссылок на блоки с освобожденной памятью
![[Pasted image 20231202223348.png]]

## 3. Compacting

упаковка свободного места в куче
![[Pasted image 20231202225844.png]]

# generations

### young gen
все новые объекты размещаются здесь. когда поколение заполняется происходит минорная сборка мусора. выжившие объекты попадают в старшее поколение.

>сначала объекты попадают в eden. когда eden заполняется триггерится минорный гц. выжившие объекты попадают в S0. при следующем проходе минорного гц выжившие попадают в S1. выжившие объекты находятся в S1 пока не достигнут определенного возраста, затем переходят в старшее поколение

### old gen
сюда объекты попадают из молодого поколения. собираются мажорным гц

### permanent gen
содержит метаданные для jvm, описывающие классы и их методы
![[Pasted image 20231202231117.png]]

## реализации гц

- Serial - сборка мусора происходит на одном потоке. упаковка происходит после каждого прохода гц. фризит все потоки когда работает.
- Parallel - много потоков для минорной сборки. один поток для мажорной и упаковки old gen'а. фризит остальные потоки.
- CMS (Concurrent Mark Sweep) - собирает old gen. обычно не копирует или упаковывает живые объекты. 
- G1 (Garbage First) - последняя реализация

## Finalization

перед тем как убить объект гц вызывает у него метод finalize(), который говорит, что на объект больше нет ссылок
> - вызывается единожды для каждого объекта
> - выброшенные в нем исключения игнорируются


# GC roots

- *Class* - классы, загруженные класс лоадером, содержит ссылки на статик переменные
- *Stack Local* - локальные переменные и параметры методов, хранящиеся в локальном стеке 
- *Active Java Threads* - все активные джава потоки
- *JNI References* - нативный код, созданный объектами для *JNI* вызовов. содержит локальные переменные, параметры *JNI* методов и глобальные *JNI* ссылки

дополнительные типы:
- объекты, используемые как мониторы для синхронизации
- специальные объекты, которые не будут собраны гц. например: экстеншн классы, системные или кастомные класс лоадеры

![[Pasted image 20240309124925.png]]
