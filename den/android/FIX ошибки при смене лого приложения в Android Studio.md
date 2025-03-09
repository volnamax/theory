[[android]]

1) Создать папку *drawable-anydpi-v26* в папке *res*
2) Выбрать тип ресурса *drawable* и источник *main*
3) Скопировать туда файлы *ic_launcher_foreground.xml* и *ic_launcher_background.xml*
4) Снести папку *drawable-v24*

[developer.android.com](https://developer.android.com/codelabs/basic-android-kotlin-training-change-app-icon?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-kotlin-unit-2-pathway-2%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-training-change-app-icon#5)

**Почему так?**

> Причина в том, что объекты заднего фона содержат градиент, который стал доступен начиная с версии Android 7.0 (API 24), поэтому он хранится в папке *drawable-v24*, в отличие от объекта переднего фона, который хранится в папке *drawable*.  Вместо того, чтобы хранить объекты переднего и заднего фонов в разных папках, перемещаем их в одну.