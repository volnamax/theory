[[kotlin]]

Создает из элементов (с одинаковыми индексами) двух списков пары. 
Можно использовать как метод *zip()*, так и инфиксную форму *zip*.

```kotlin
for ( (i, j) in nums.indices zip nums.indices.reversed()) {  
	println("$i - $j")  
}
```
Для размера *nums* = 5 выведет:

0 - 5
1 - 4
2 - 3
3 - 2
4 - 1
5 - 0

[kotlin.lang](https://kotlinlang.ru/docs/collection-transformations.html)