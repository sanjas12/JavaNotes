---
tags:
  - StackTrace
---
**Стек-трейс (Stack Trace)** — это отчет, который показывает последовательность вызовов методов в приложении на языке программирования (например, Java) в момент возникновения ошибки или исключения. Стек-трейс содержит информацию о том, какие методы были вызваны, в каком порядке, и на каких строках кода возникла ошибка.

![[Pasted image 20240913011538.png]]


## Вывод стек-трейса при обработке ошибок
```java
try
{
   // тут может возникнуть исключение
}
catch(Exception e)
{
   StackTraceElement[] methods = e.getStackTrace()
}
```
## Вывод стек-трейса через Thread
Статический метод `currentThread()` класса `Thread` возвращает ссылку на объект типа `Thread`, который содержит информацию о текущей нити (о текущем потоке выполнения).
У этого объекта `Thread` есть метод `getStackTrace()`, который возвращает массив элементов `StackTraceElement`, каждый из которых содержит информацию об одном методе. Все элементы вместе и образуют stack trace.
```java
public class Main
{
   public static void main(String[] args)
   {
      test();
   }

   public static void test()
   {
      Thread current = Thread.currentThread();
      StackTraceElement[] methods = current.getStackTrace();

      for(var info: methods)
         System.out.println(info);
   }
}
```

https://javarush.com/groups/posts/2972-stack-trace-i-s-chem-ego-edjat
