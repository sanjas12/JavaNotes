---
tags: Java/ООП
---
## Вызов конструктора
Если в базовом классе определены конструкторы, то в конструкторе производного класса нужно вызвать один из конструкторов базового класса с помощью ключевого слова **super**.
## Получение экземпляра суперкласса
В подклассе можем напрямую обращаться к свойствам и методам, унаследованным от родительского класса, но, если свойства или методы подкласса имеют одинаковое имя (затенение атрибутов, переопределение метода), их необходимо различать, прежде чем к ним можно будет получить доступ.

Иногда бывает нужно не заменить метод родительского класса на свой при переопределении метода, а лишь немного дополнить его. Было бы классно, если бы мы могли в нашем методе вызвать такой же метод родительского класса, а потом еще выполнить какой-то свой код. Ну или сначала выполнить свой код, а потом вызвать метод родительского класса. И такая возможность в Java есть. Вызов метода именно родительского класса делает так:
```
super.метод(параметры);
```

В Java ключевое слово `super` используется для обращения к членам суперкласса из подкласса. Его можно использовать в трех основных контекстах:

### 1. **Вызов конструктора суперкласса**

С помощью `super` можно явно вызвать конструктор суперкласса. Это должно быть первой строкой в конструкторе подкласса.

```
class Animal {
    Animal(String name) {
        System.out.println("Animal is: " + name);
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name); // Вызывает конструктор суперкласса
        System.out.println("Dog is: " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Bulldog");
    }
}
```

### 2. **Обращение к полям суперкласса**

Если в подклассе и суперклассе есть поля с одинаковыми именами, `super` позволяет явно обратиться к полю суперкласса.
```
class Animal {
    String name = "Animal";
}

class Dog extends Animal {
    String name = "Dog";

    void printNames() {
        System.out.println("Name in subclass: " + name);
        System.out.println("Name in superclass: " + super.name);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.printNames();
    }
}


Animal is: Bulldog
Dog is: Bulldog

```

### 3. Обращение к методам суперкласса

Если метод переопределен в подклассе, super позволяет вызвать его оригинальную реализацию из суперкласса.

```
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        super.sound(); // Вызывает метод суперкласса
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();
    }
}

Animal makes a sound 
Dog barks

```