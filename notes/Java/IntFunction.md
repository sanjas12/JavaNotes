`IntFunction<R>` - функция, получающая на вход `Integer` и возвращающая на выходе экземпляр класса `R`;

```java
ArrayList<Integer> integers = new ArrayList<Integer>();
Collections.addAll(integers, 1000, 2000, 3000);

integers.toArray(Integer[]::new); //эту строчку перепишем ниже другими способами
```

**2)** с лямбда-выражением строка будет выглядеть так:

```java
integers.toArray(i -> new Integer[i]);
```

**3)** а если использовать анонимный класс, то вид будет следующий:

```java
integers.toArray(new IntFunction<Integer[]>() {
            @Override
            public Integer[] apply(int i) {
                return new Integer[i];
            }
        });
```

