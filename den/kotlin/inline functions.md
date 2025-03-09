[[kotlin]]

Inline функции следует использовать только в случае передачи в функцию параметров функционального типа.

Основная разница между вызовами inline функции и обычной функции в том, что для вызова обычной функции в Java создается анонимный класс, реализующий нашу лямбду и его экземпляр передается в обычную функцию.

```kotlin
inline fun inlineFun(body: () -> String) {
	println("inline func code, " + body.invoke())    
}    

fun testInline() {       
	inlineFun { "external code" }    
}
```

```java
public final void inlineFun(Function0 body) {        
	String var2 = "inline func code, " + (String)body.invoke();       
	System.out.println(var2);    
}


public final void testInline() {        
	String var4 = (new StringBuilder())
			.append("inline func code, ")                
			.append("external inline code")                
			.toString();        
	System.out.println(var4);    
}
```


```kotlin
private inline fun inlineFun(body: () -> String) {
	println("inline func code, " + body.invoke())    
}    

fun testInline() {        
	inlineFun { "external inline code" }    
}    

private fun regularFun(body: () -> String) {        
	println("regular func code, " + body.invoke())    
}    

fun testRegular() {        
	regularFun { "external regular code" }    
}
```

```java
public final void testInline() {        
	String var4 = (new StringBuilder())                
				.append("inline func code, ")                
				.append("external inline code")                
				.toString();        
	System.out.println(var4);    
}    

public final void testRegular() {        
	Function0 body = (Function0)(new Function0() {            
		public final String invoke() {                
			return "external regular code";            
		}        
	});        
	this.regularFun(body);    
}
```

> _Создание инстанса анонимного класса в Java — это достаточно затратная операция и профит inline функций именно в этом._

**_Inline функции позволяют исключить создание анонимных классов для передачи лямбд в параметры функции._**

# noinline & crossinline

```kotlin
private inline fun crossInlineFun(body: () -> String) {
	val func = {
	    "crossInline func code, " + body.invoke()
	}        
	regularFun(func)    
}
```

Если вы напишите такой код, то получите ошибку компилятора. Это происходит из-за того, что компилятор не может заинлайнить вашу функцию, так как она использует входящую лямбду **_body_** внутри локальной лямбды **_func_**. В случае инлайнинга у нас просто нет анонимного класса для **_body_** и мы не можем его передать в локальную лямбду.

Но мы можем пометить наш параметр как **noinline** и в этом случае все скомпилируется. Когда мы помечаем параметр как **noinline**, то для него будет создаваться анонимный класс и его можно будет передать в локальную лямбду.

```kotlin
private inline fun crossInlineFun(noinline body: () -> String) {
	val func = {
	    "crossInline func code, " + body.invoke()       
	}        
	regularFun(func)    
}    

fun testCrossInline() {
	crossInlineFun { "external code" }    
}
```

```java
public final void testCrossInline() {
	Function0 body = (Function0)(new Function0() {
	            public final String invoke() {
	                return "external code";            
	            }        
	});        
	
	Function0 func = (Function0)(new Function0() {
	            public final String invoke() {
		            return "crossInline func code, " + (String)body.invoke();      
		        }        
	});        
	
	regularFun(func);    
}
```

Как видите функция crossInlineFun встроилась в место вызова, но так как параметр помечен как noinline, то мы потеряли весь профит от инлайнинга. У нас создается два анонимных класса и второй анонимный класс **_func_** вызывает из себя первый анонимный класс **_body_**.

Теперь давайте пометим наш параметр как **crossinline** и посмотрим как изменится Java код.

```kotlin
private inline fun crossInlineFun(crossinline body: () -> String) {
	val func = {
	    "crossInline func code, " + body.invoke()        
	}        
	
	regularFun(func)    
}    

fun testCrossInline() {
	crossInlineFun { "external code" }    
}
```

```java
public final void testCrossInline() {
	Function0 func = (Function0)(new Function0() {
	    public final String invoke() {
		        return (new StringBuilder())                 
			        .append("crossInline func code, ")
			        .append("external code")
			        .toString();
		}        
	});        
	
	regularFun(func);    
}
```

Как видите, в случае crossinline мы имеем не два анонимных класса, а только один, который объединяет в себе код inline функции и код внешней лямбды.

> В случае использования входящего параметра функционального типа в локальной лямбде, добавление **crossinline** позволяет исключить создание дополнительного анонимного класса. Вместо двух анонимных классов будет создан только один.

