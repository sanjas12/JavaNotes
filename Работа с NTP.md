## 1. Просмотр текущей даты и времени
```cmd
time /t      # только время
date /t      # только дата
time         # время + запрос на изменение
date         # дата + запрос на изменение
```
## 2. Установка даты и времени вручную

```cmd
date 2025-05-20
time 14:30:00
```

Требуются права администратора.

## 3. Настройка синхронизации времени через NTP
### 1. **Статус службы времени Windows:**
```cmd
w32tm /query /status
```
Покажет:
- **Source** - источник синхронизации (Local CMOS Clock, или NTP-сервер)
- **Last Successful Sync Time** - время последней синхронизации
- **Poll Interval** - интервал опроса

### 2. **Проверка конфигурации NTP (Посмотреть текущий NTP-сервер):
```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters" /v NtpServer
```

### 3. **Более подробная информация:**
```cmd
w32tm /query /configuration
```

### 4. **Проверка синхронизации с конкретным сервером:**
```cmd
w32tm /stripchart /computer:time.windows.com /dataonly /samples:5
```

### 3.1 Посмотреть текущий NTP-сервер

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters" /v NtpServer
```

### 3.2 Изменить NTP-сервер (например, на `time.windows.com`)

```cmd
w32tm /config /manualpeerlist:"time.windows.com" /syncfromflags:manual /reliable:yes /update
```

Или на российские серверы:

```cmd
w32tm /config /manualpeerlist:"ntp1.vniiftri.ru ntp2.vniiftri.ru" /syncfromflags:manual /reliable:yes /update
```

### 3.3 Запустить синхронизацию вручную

```cmd
w32tm /resync
```

Если служба времени не запущена:

```cmd
net start w32time
```

### 3.4 Получить статус синхронизации

```cmd
w32tm /query /status
```

Покажет источник времени, последнюю синхронизацию, текущее смещение.

## 4. Работа со временем в скриптах

### Получить текущую дату в формате ГГГГ-ММ-ДД

```cmd
for /f "tokens=1-3 delims=. " %a in ('date /t') do @echo %c-%b-%a
```

### Получить текущее время без секунд

```cmd
echo %TIME:~0,5%
```

### Создать штамп времени для имени файла

```cmd
set DATETIME=%DATE:~6,4%%DATE:~3,2%%DATE:~0,2%_%TIME:~0,2%%TIME:~3,2%%TIME:~6,2%
echo %DATETIME%
```

## 5. Управление службой времени Windows

```cmd
sc query w32time          # статус службы
sc stop w32time           # остановить
sc start w32time          # запустить
sc config w32time start=auto   # автозапуск
```

## 6. Пример полной настройки NTP

```cmd
:: Запуск от имени администратора
net stop w32time
w32tm /config /manualpeerlist:"pool.ntp.org" /syncfromflags:manual /reliable:yes /update
net start w32time
w32tm /resync
w32tm /query /status
```

## 7. Проверка смещения времени без синхронизации

```cmd
w32tm /stripchart /computer:time.windows.com /samples:5 /dataonly
```

Покажет разницу между системным временем и NTP-сервером в секундах.

## Важно

- Для изменения параметров NTP и синхронизации нужны **права администратора**
- Если `w32tm /resync` возвращает ошибку, перезапустите службу `w32time`
- В Windows 10/11 по умолчанию синхронизация происходит раз в несколько дней, можно ускорить через `w32tm /resync`


