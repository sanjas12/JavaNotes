## 1. Date (Устаревший)
1. `import java.util.Date;` устаревший класс. Из документации надо использовать Calendar

https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Date.html

Для форматирования вывода даты

```java
import java.text.SimpleDateFormat;

SimpleDateFormat formatter = new SimpleDateFormat("Y-MMM-dd");
String str = formatter.format(my_date);
System.out.println(str);
```
## 2. Calendar
2. `import java.util.Calendar;`

https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Calendar.html

```java
Calendar calendar = Calendar.getInstance();

System.out.println(calendar.getTime());

int month = calendar.get(Calendar.MONTH); // получение месяца

calendar.set(Calendar.MONTH, значение); // изменения фрагмента даты

```

Метод `add()`  позволяет проводить с датой более умные операции. Например, добавить к дате год, месяц или несколько дней. Или отнять.

`roll()` аналогичен методу `add()`, но любые изменения с его помощью затрагивают один параметр, остальные остаются неизменными. (если менять только месяц, не затрагивая год )

### 3. Time
3. `java.time` (**начиная с Java 8**)

Начиная с Java 8, пакет `java.time` предоставляет современный и удобный способ работы с датами и временем. Этот API был разработан для устранения недостатков классов `Date` и `Calendar` и основан на библиотеке Joda-Time.

**Основные классы `java.time`:**
- `LocalDate` — только дата (без времени).
- `LocalTime` — только время (без даты).
- `LocalDateTime` — дата и время.
- `ZonedDateTime` — дата и время с учетом часового пояса.
- `Instant` — точное время в формате времени эпохи (Unix timestamp).
- `Duration` и `Period` — работа с промежутками времени.
Все объекты этих классов — `immutable`: их нельзя изменить после создания


**Форматирование и парсинг** делайте с помощью `DateTimeFormatter`, который обеспечивает гибкость и локализацию.
```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
public class Main {
    public static void main(String[] args) {
        // Получение текущих даты и времени
        LocalDateTime now = LocalDateTime.now();
        System.out.println("Текущие дата и время: " + now);

        // Форматирование даты
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd.MM.yyyy HH:mm");
        String formattedDate = now.format(formatter);
        System.out.println("Отформатированная дата и время: " + formattedDate);

        // Парсинг строки в дату
        LocalDate parsedDate = LocalDate.parse("2024-11-18");
        System.out.println("Парсинг даты: " + parsedDate);
    }
```


Пакет `java.time.zone` содержит классы для работы с часовыми поясами (time zones). Он содержит такие классы как `TimeZone` и `ZonedDateTime`.