```bash
#!/bin/bash

# Определяем адрес вебсайта
WEBSITE="example.com"

# Файл для логирования результатов
LOG_FILE="/var/log/site_status.log"

# Проверяем доступность сайта с помощью ping
WEBSITES=("example.com" "google.com" "stackoverflow.com")
for SITE in ${WEBSITES[@]}; do
    if ping -c 1 $SITE &> /dev/null; then
        echo "$(date): $SITE is reachable" >> $LOG_FILE
    else
        echo "$(date): $SITE is unreachable" >> $LOG_FILE
		echo "$WEBSITE is down!" | mail -s "Website Check Alert" your_email@example.com
    fi
done


# Пути к лог-файлу и папке для архивов
LOG_FILE="/var/log/my_log.log"
LOG_DIR="/var/log"
MAX_SIZE=1048576  # 1 MB (в байтах)

# Проверяем, существует ли лог-файл и превышает ли он 1 MB
if [ -f "$LOG_FILE" ] && [ $(stat -c%s "$LOG_FILE") -ge $MAX_SIZE ]; then
    TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
    ARCHIVE_FILE="${LOG_DIR}/my_log_$TIMESTAMP.log"

    # Переименовываем лог-файл
    mv "$LOG_FILE" "$ARCHIVE_FILE"
    echo "Лог-файл был архивирован как $ARCHIVE_FILE"

    # Создаем новый пустой лог-файл
    touch "$LOG_FILE"
fi

```

Сделайте его исполняемым:

```bash
chmod +x site_check.sh
```

Откройте редактор `crontab`:

```bash
crontab -e
```
Добавьте следующую строку:

```bash
*/5 * * * * /path/to/site_check.sh
```


- Если вы запускаете скрипт от имени обычного пользователя, возможно, у него недостаточно прав. Выполните скрипт с `sudo` или добавьте права на выполнение.
    
- **Файл логов не создаётся**
    
    Убедитесь, что путь к файлу логов (`/var/log/site_status.log`) указан правильно и у вашего пользователя есть права на запись в эту директорию. Если файла нет, создайте его вручную с помощью:


Проверьте, работает ли служба `cron`:

```bash
sudo systemctl status cron
```