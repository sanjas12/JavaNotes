Вот все основные способы запуска Python и ключи (флаги/опции) для командной строки Windows (cmd):

## **Базовый синтаксис**
```cmd
python [ключи] [-c команда | -m модуль | файл] [аргументы]
```

## **Основные ключи Python**

### **Информационные**
```cmd
python --version    # или -V - версия Python
python -VV          # детальная версия (с компилятором)
python -h           # --help - справка по всем ключам
```

### **Управление выполнением**
```cmd
python -c "print('hello')"    # выполнить строку как код Python
python -m pip install requests  # запустить модуль как скрипт
python script.py               # запустить файл
python -i script.py            # запустить и войти в интерактивный режим
```

### **Оптимизация и отладка**
```cmd
python -O          # оптимизация (удаляет assert, __debug__)
python -OO         # оптимизация + удаление docstrings
python -d          # отладочный режим (вывод отладочных сообщений)
python -q          # не показывать приветствие в интерактивном режиме
python -v          # подробный режим (импорт модулей)
python -vv         # ещё более подробный режим
python -X tracemalloc  # включить трассировку выделения памяти
```

### **Предупреждения**
```cmd
python -W ignore   # игнорировать предупреждения
python -W error    # превратить предупреждения в ошибки
python -W default  # показывать каждое предупреждение один раз
python -W all      # показывать все предупреждения
```

### **Пути и окружение**
```cmd
python -E          # игнорировать переменные окружения PYTHON*
python -s          # не добавлять site-packages
python -S          # отключить импорт site при запуске
python -P          # не добавлять потенциально небезопасные пути
```

### **Изоляция**
```cmd
python -I          # изолированный режим ( -E -P -s)
```

### **Буферизация вывода**
```cmd
python -u          # несвязанный (unbuffered) ввод/вывод
```

### **Хэширование**
```cmd
python -R          # рандомизировать хэши (включено по умолчанию)
```

## **Практические примеры**

```cmd
# Запуск скрипта с аргументами
python script.py arg1 arg2 --option value

# Запуск с оптимизацией
python -O script.py

# Запуск и интерактивный режим после выполнения
python -i script.py

# Проверка синтаксиса без выполнения
python -m py_compile script.py

# Запуск встроенного веб-сервера
python -m http.server 8000

# Выполнение многострочного кода
python -c "for i in range(5): print(i)"

# Показать все встроенные модули
python -m help("modules")

# Запуск с отладкой памяти
python -X tracemalloc=5 script.py
```

## **Переменные окружения PYTHON* (аналоги ключей)**

```cmd
set PYTHONOPTIMIZE=1    # аналог -O
set PYTHONDEBUG=1       # аналог -d
set PYTHONVERBOSE=1     # аналог -v
set PYTHONUNBUFFERED=1  # аналог -u
set PYTHONHASHSEED=random  # аналог -R
```

## **Получить полный список**
```cmd
python -h
python --help
```

Это основные ключи для Python в Windows cmd. Некоторые ключи могут отличаться в зависимости от версии Python (2.x vs 3.x).