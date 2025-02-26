
``` bash
#!/bin/bash
echo "Как тебя зовут?"
#read FILE
#echo "Привет, $NAME!"
#echo "Текущая дата и время:"
#date

CURRENT_DATE=$(date)
echo "Сейчас: $CURRENT_DATE" 
FILE_NAME="test.sh"
FILE_NAME2="test2.sh"


# Директория, где будут созданы файлы (по умолчанию текущая)
DIR="./"

# Цикл для создания 10 файлов
for i in {1..10}
do
  	FILE="${DIR}file${i}.txt"
	#echo "Создано 10 файлов в директории ${DIR}"
	if [ -f "$FILE" ]; then
	    echo "Файл $FILE существует."
	else
	    touch "$FILE"
	    echo  "$FILE создан."
	fi
done

for FILE in *; do
    echo "Обрабатывается файл: $FILE"
done

if [ -f "$FILE_NAME2" ]; then
    echo "Файл $FILE_NAME существует."
else
    echo "Файл $FILE_NAME2 не существует."
fi

SITES=("example.com" "google.com" "nonexistent.website")
for SITE in ${SITES[@]}; do
    if ping -c 1 $SITE &> /dev/null; then
        echo "$SITE доступен."
    else
        echo "$SITE недоступен."
    fi
done
```
