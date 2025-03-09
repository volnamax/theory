[[java]]

## Checked

эксепшены, чекнутые во время компиляции. если метод выкидывает *checked exception*, он должен обрабатывать исключение (try catch), либо уточнять тип исключения, используя *throws*
```java
private static void checkedExceptionWithThrows() throws FileNotFoundException {
    File file = new File("not_existing_file.txt");  
    FileInputStream stream = new FileInputStream(file);  
}
```

## Unchecked

происходят в рантайме. например деление на 0

## Иерархия

![[Pasted image 20231124105901.png]]

*Errors* - непредвиденные обстоятельства, которые не могут быть обработаны программой. например, stackoverflow