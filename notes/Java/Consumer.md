Интерфейс `Consumer<Тип>` (Consumer == Потребитель), который содержит метод `accept(Тип obj)`. Зачем же нужен этот интерфейс?

В Java 8 у коллекций появился метод `forEach()`, который позволяет для каждого элемента коллекции выполнить какое-нибудь действие. И вот для передачи действия в метод `forEach()` как раз и используется [[функциональный интерфейс]] `Consumer<T>`.

```java
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "Привет", "как", "дела?");

list.forEach( (s) -> System.out.println(s) );  \\ лямбда-выражение
```
Компилятор преобразует этот код в код:
```java
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "Привет", "как", "дела?");

\\ запись с использованием анонимного класса
list.forEach(new Consumer<String>()
{
   public void accept(String s)
   {
      System.out.println(s);
   }
});                                   
```