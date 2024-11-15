`import java.util.Date;

https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Date.html

Для форматирования вывода даты

```java
import java.text.SimpleDateFormat;

SimpleDateFormat formatter = new SimpleDateFormat("Y-MMM-dd");
String str = formatter.format(my_date);
System.out.println(str);
```