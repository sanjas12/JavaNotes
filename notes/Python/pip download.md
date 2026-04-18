Отличная команда! Только небольшие исправления по синтаксису путей в Windows:

## Правильные варианты записи пути:

```bash
# Вариант 1: Обратные слеши (Windows стиль)
pip download -r requirements.txt -d f:\temp\python_Library

# Вариант 2: Прямые слеши (работает в Windows)
pip download -r requirements.txt -d f:/temp/python_Library

# Вариант 3: Двойные обратные слеши (для экранирования)
pip download -r requirements.txt -d f:\\temp\\python_Library

# Вариант 4: С кавычками (если есть пробелы в пути)
pip download -r requirements.txt -d "f:\temp\python_Library"
```

## Полный процесс подготовки offline пакетов:

```bash
# 1. Создаем директорию (если её нет)
mkdir -p f:/temp/python_Library

# 2. Скачиваем все зависимости
pip download -r requirements.txt -d f:/temp/python_Library

# 3. Проверяем, что скачалось
ls -la f:/temp/python_Library/

# 4. Дополнительно: скачать с конкретной платформой (если нужно)
pip download -r requirements.txt -d f:/temp/python_Library \
    --platform win_amd64 \
    --python-version 3.8 \
    --no-deps
```

## Улучшенная версия скрипта с проверкой:

```bash
#!/bin/bash

cd "$(dirname "$0")/.." || exit 1

# Конфигурация - используем единый формат пути
LOCAL_PACKAGES_DIR="f:/temp/python_Library"

check_internet() {
    if command -v curl &>/dev/null; then
        curl -s --connect-timeout 3 https://8.8.8.8 >/dev/null 2>&1
    elif command -v wget &>/dev/null; then
        wget -q --timeout=3 https://8.8.8.8 -O /dev/null 2>&1
    else
        if [[ "$OS" == "Windows_NT" ]]; then
            ping -n 1 -w 3000 8.8.8.8 >/dev/null 2>&1
        else
            ping -c 1 -W 3 8.8.8.8 >/dev/null 2>&1
        fi
    fi
    return $?
}

# Функция для проверки наличия всех пакетов
check_packages_available() {
    local packages_dir="$1"
    local missing=()
    
    while IFS= read -r package; do
        [[ -z "$package" || "$package" =~ ^# ]] && continue
        
        # Извлекаем имя пакета без версии
        pkg_name=$(echo "$package" | sed -E 's/([a-zA-Z0-9_-]+).*/\1/')
        
        # Ищем файл пакета
        if ! find "$packages_dir" -type f -name "${pkg_name}*" 2>/dev/null | grep -q .; then
            missing+=("$pkg_name")
        fi
    done < requirements.txt
    
    if [ ${#missing[@]} -eq 0 ]; then
        return 0
    else
        echo "❌ Отсутствуют пакеты: ${missing[*]}"
        return 1
    fi
}

if ! check_internet; then
    echo "❌ Интернет недоступен — устанавливаем локально через pip"
    
    echo "Cleaning previous venv..."
    rm -rf .venv
    
    python -m venv .venv
    
    # Активация
    if [[ -f ".venv/Scripts/activate" ]]; then
        source .venv/Scripts/activate
    else
        source .venv/bin/activate
    fi
    
    if [ ! -f "requirements.txt" ]; then
        echo "❌ Файл requirements.txt не найден"
        exit 1
    fi
    
    # Проверяем наличие локальной директории
    if [ ! -d "$LOCAL_PACKAGES_DIR" ]; then
        echo "❌ Директория с пакетами не найдена: $LOCAL_PACKAGES_DIR"
        echo "Создайте её и скачайте пакеты на машине с интернетом:"
        echo "  mkdir -p $LOCAL_PACKAGES_DIR"
        echo "  pip download -r requirements.txt -d $LOCAL_PACKAGES_DIR"
        exit 1
    fi
    
    # Проверяем наличие всех пакетов
    if ! check_packages_available "$LOCAL_PACKAGES_DIR"; then
        echo ""
        echo "Чтобы скачать недостающие пакеты (на машине с интернетом):"
        echo "  pip download -r requirements.txt -d $LOCAL_PACKAGES_DIR"
        exit 1
    fi
    
    echo "Устанавливаем из $LOCAL_PACKAGES_DIR..."
    
    # Установка без интернета
    pip install --no-index \
                --find-links "$LOCAL_PACKAGES_DIR" \
                -r requirements.txt
    
    if [ $? -eq 0 ]; then
        echo "✅ .venv создан через pip (локальная установка)"
    else
        echo "❌ Ошибка при установке"
        exit 1
    fi
    
    exit 0
fi

echo "✅ Интернет доступен"

# ... остальной код для установки с интернетом ...
```

## Полезные команды для отладки:

```bash
# Проверить, какие файлы есть в директории
ls -la "f:/temp/python_Library/"

# Проверить, есть ли конкретный пакет
find "f:/temp/python_Library" -name "*contourpy*"

# Посмотреть зависимости в requirements.txt
cat requirements.txt

# Скачать только конкретный пакет
pip download contourpy==1.0.7 -d f:/temp/python_Library
```

**Важно:** После выполнения `pip download -r requirements.txt -d f:/temp/python_Library` убедитесь, что в директории появился файл `contourpy-1.0.7-*.whl` или `contourpy-1.0.7.tar.gz`. Если его нет, значит пакет не скачался - проверьте интернет и правильность имени пакета в requirements.txt.