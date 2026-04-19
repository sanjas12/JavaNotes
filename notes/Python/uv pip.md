В `uv` есть несколько способов посмотреть установленные пакеты, в зависимости от того, что именно вы хотите увидеть.

## 1. **Стандартный способ** (как pip list)

```bash
# Список всех установленных пакетов в текущем окружении
uv pip list

# С пакетами в виде requirements формата
uv pip freeze

# Подробная информация (версии, пути, зависимости)
uv pip list --verbose
```

## 2. **Дерево зависимостей** (очень полезно!)

```bash
# Показать иерархию зависимостей
uv pip tree

# Ограничить глубину
uv pip tree --depth 2

# Показать только определенный пакет
uv pip tree --package requests

# В обратном порядке (кто зависит от пакета)
uv pip tree --reverse
```

Пример вывода `uv pip tree`:
```
my-project v0.1.0
├── fastapi v0.104.1
│   ├── starlette v0.27.0
│   │   └── anyio v3.7.1
│   ├── pydantic v2.5.0
│   └── uvicorn v0.24.0
└── requests v2.31.0
    ├── urllib3 v2.1.0
    ├── certifi v2023.11.17
    └── charset-normalizer v3.3.2
```

## 3. **Показать устаревшие пакеты**

```bash
# Какие пакеты можно обновить
uv pip list --outdated

# Более детально
uv pip list --outdated --format=json
```

## 4. **Информация о конкретном пакете**

```bash
# Показать информацию о пакете
uv pip show requests

# Подробно
uv pip show --verbose requests

# Только зависимости пакета
uv pip show --requires requests
```

## 5. **Разные форматы вывода**

```bash
# JSON формат (удобно для скриптов)
uv pip list --format=json

# Столбцы (по умолчанию)
uv pip list --format=columns

# Freeze формат (как requirements.txt)
uv pip list --format=freeze

# Только имена пакетов
uv pip list --format=freeze | cut -d= -f1
```

## 6. **Проверка состояния окружения**

```bash
# Проверить, нет ли конфликтов
uv pip check

# Показать все пакеты с версиями
uv pip list --exclude-editable

# Включая редактируемые установки
uv pip list --include-editable
```

## 7. **Для разных окружений**

```bash
# Если у вас несколько виртуальных окружений
# Указать конкретное окружение
uv pip list --python .venv/bin/python

# Или через путь
UV_PYTHON=/path/to/venv/bin/python uv pip list
```

## 8. **Через uv run** (без активации окружения)

```bash
# Из любой директории проекта
uv run pip list
uv run pip freeze
uv run pip tree
```

## 9. **Продвинутые фильтры**

```bash
# Найти пакеты по имени
uv pip list | grep requests

# Исключить стандартные пакеты
uv pip list --exclude pip --exclude setuptools

# Только пакеты, не входящие в стандартную библиотеку
uv pip list --local
```

## 10. **Сравнение с pip**

| Действие | pip | uv |
|----------|-----|-----|
| Список пакетов | `pip list` | `uv pip list` |
| Заморозить версии | `pip freeze` | `uv pip freeze` |
| Дерево зависимостей | `pipdeptree` (нужно устанавливать) | `uv pip tree` (встроено) |
| Информация о пакете | `pip show` | `uv pip show` |
| Устаревшие пакеты | `pip list --outdated` | `uv pip list --outdated` |

## Быстрые примеры для повседневного использования

```bash
# 1. Просто посмотреть, что установлено
uv pip list

# 2. Увидеть структуру зависимостей (самое полезное!)
uv pip tree

# 3. Сохранить список в файл
uv pip freeze > requirements.txt
uv run pip freeze > requirements.txt
uv pip freeze --python .venv > requirements.txt

# 4. Найти, какой пакет тянет проблемную зависимость
uv pip tree --reverse | grep problematic-package

# 5. Посмотреть версию конкретного пакета
uv pip show requests | grep Version
```

## Полезные алиасы для ~/.bashrc или ~/.zshrc

```bash
# Добавьте в конфиг шелла для быстрого доступа
alias uv-list='uv pip list'
alias uv-tree='uv pip tree'
alias uv-outdated='uv pip list --outdated'
alias uv-show='uv pip show'
```

Теперь вы можете использовать `uv pip tree` — это моя любимая команда, которой очень не хватает в стандартном pip!

# 📚 Полный гайд по `uv pip` — современному пакетному менеджеру

`uv` — это сверхбыстрый менеджер пакетов и виртуальных окружений на Rust (замена pip, pip-tools, virtualenv, poetry). `uv pip` — это подкоманда, реализующая интерфейс, совместимый с классическим `pip`.

## 🚀 Установка uv

```bash
# Linux/macOS
curl -LsSf https://astral.sh/uv/install.sh | sh

# Или через pip
pip install uv

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Проверка установки
uv --version
```

## 📦 Основные команды `uv pip`

### 1. **Установка пакетов**

```bash
# Установка одного пакета
uv pip install requests

# Установка конкретной версии
uv pip install "requests==2.31.0"

# Установка нескольких пакетов
uv pip install requests numpy pandas

# Установка из requirements.txt
uv pip install -r requirements.txt

# Установка с опциями
uv pip install --verbose package  # Подробный вывод
uv pip install --no-deps package   # Без зависимостей
uv pip install --pre package       # Включая пре-релизы
uv pip install --constraint constraints.txt  # С ограничениями
```

### 3. **Просмотр информации о пакетах**

```bash
# Список установленных пакетов
uv pip list
uv pip list --outdated  # Устаревшие пакеты
uv pip list --format=json  # JSON формат
uv pip list --editable  # Editable установки

# Информация о конкретном пакете
uv pip show requests
uv pip show --files requests  # Показать все файлы пакета

# Проверка зависимостей
uv pip check  # Проверить конфликты зависимостей
uv pip tree   # Дерево зависимостей (аналог pipdeptree)
```

### 4. **Замораживание зависимостей**

```bash
# Вывести все установленные пакеты
uv pip freeze
uv pip freeze > requirements.txt

# Без хэшей
uv pip freeze --no-hashes

# Только прямые зависимости (не транзитивные)
uv pip freeze --exclude-editable
```

### 5. **Удаление пакетов**

```bash
# Удаление пакета
uv pip uninstall requests

# Удаление нескольких
uv pip uninstall requests numpy pandas

# Удаление всех пакетов из файла
uv pip uninstall -r requirements.txt

# Автоматическое подтверждение
uv pip uninstall -y requests
```

## 🎯 Продвинутое использование

### Работа с индексами пакетов

```bash
# Использование альтернативного индекса
uv pip install torch --index-url https://download.pytorch.org/whl/cpu

# Несколько индексов
uv pip install package \
  --index-url https://pypi.org/simple \
  --extra-index-url https://my-private-pypi.com/simple

# Только из указанного индекса (не из PyPI)
uv pip install --index-url https://my-pypi.com/simple --no-index package
```

### Кэширование

```bash
# Очистка кэша
uv cache clean
uv cache clean requests  # Очистить кэш конкретного пакета

# Информация о кэше
uv cache dir  # Показать директорию кэша
uv cache size # Размер кэша

# Отключение кэша
uv pip install --no-cache package
```

### Виртуальные окружения

```bash
# Создание виртуального окружения
uv venv
uv venv myenv
uv venv --python 3.11  # Конкретная версия Python

# Активация (разные ОС)
source .venv/bin/activate      # Linux/macOS
.venv\Scripts\activate          # Windows
source .venv/bin/activate.fish  # Fish shell

# Установка пакетов в конкретное окружение
uv pip install --python .venv/bin/python requests
uv pip install --venv .venv requests  # Короче

# Запуск скрипта в окружении
uv run python script.py
uv run --no-sync python script.py  # Без синхронизации зависимостей
```

## 🔄 Миграция с pip

| pip команда | uv pip команда | Примечание |
|------------|----------------|------------|
| `pip install` | `uv pip install` | ⚡ В 10-100x быстрее |
| `pip freeze` | `uv pip freeze` | Совместимый вывод |
| `pip list` | `uv pip list` | Дополнительные форматы |
| `pip show` | `uv pip show` | Полная совместимость |
| `pip download` | `uv pip download` | + поддержка платформ |
| `pip uninstall` | `uv pip uninstall` | Полная совместимость |
| `pip check` | `uv pip check` | Быстрее и точнее |

## 🏭 Реальные сценарии использования

### Сценарий 1: Быстрая установка в CI/CD

```bash
#!/bin/bash
# В GitHub Actions, GitLab CI и т.д.

# Установка uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Создание окружения и установка зависимостей (всё в одной команде!)
uv venv
uv pip install -r requirements.txt

# Кэширование на уровне CI
uv cache  # Управление кэшем
```

### Сценарий 2: Офлайн-разработка

```bash
# На машине с интернетом
mkdir offline_packages
uv pip download -r requirements.txt -d offline_packages
tar -czf offline_packages.tar.gz offline_packages/

# На офлайн-машине
tar -xzf offline_packages.tar.gz
uv venv
uv pip install --no-index --find-links ./offline_packages -r requirements.txt
```

### Сценарий 3: Многоступенчатая установка

```bash
# Установка с разными зависимостями для разных окружений
uv pip install -r requirements/base.txt
uv pip install -r requirements/dev.txt --no-deps  # Без проверки зависимостей
uv pip install -r requirements/test.txt
```

### Сценарий 4: Сборка Docker-образов

```dockerfile
FROM python:3.11-slim

# Установка uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# Копируем зависимости
COPY requirements.txt .

# Установка (кэшируется на слое Docker)
RUN uv venv && uv pip install --no-cache -r requirements.txt

# Копируем приложение
COPY . .

CMD ["uv", "run", "python", "app.py"]
```

### Сценарий 5: Работа с приватными репозиториями

```bash
# Установка из приватного Git репозитория
uv pip install git+https://github.com/user/private-repo.git

# С SSH ключом
uv pip install git+ssh://git@github.com/user/private-repo.git

# Конкретный бранч или тег
uv pip install git+https://github.com/user/repo.git@main
uv pip install git+https://github.com/user/repo.git@v1.0.0
```

## ⚙️ Расширенная конфигурация

### Файл конфигурации `pyproject.toml`

```toml
[tool.uv]
# Индексы пакетов
index-url = "https://pypi.org/simple"
extra-index-url = ["https://my-private-pypi.com/simple"]

# Кэширование
cache-dir = "./.uv-cache"
no-cache = false

# Дополнительные настройки
native-tls = true  # Использовать системный TLS
offline = false    # Офлайн-режим
```

### Переменные окружения

```bash
# Индексы
export UV_INDEX_URL="https://pypi.org/simple"
export UV_EXTRA_INDEX_URL="https://my-pypi.com/simple"

# Кэш
export UV_CACHE_DIR="$HOME/.uv-cache"
export UV_NO_CACHE="true"

# Таймауты
export UV_HTTP_TIMEOUT="30"  # Таймаут в секундах

# Прокси
export HTTP_PROXY="http://proxy:8080"
export HTTPS_PROXY="https://proxy:8080"
```

## 🎨 Полезные трюки

```bash
# Установка пакетов из локальных wheel-файлов
uv pip install ./downloads/*.whl

# Установка editable режим (разработка)
uv pip install -e ./my_package

# Создание requirements.txt с хэшами для безопасности
uv pip freeze --hash > requirements.txt

# Установка только определённых зависимостей для Python версии
uv pip install "package; python_version < '3.9'"

# Параллельная загрузка (автоматически)
uv pip download -r requirements.txt  # Уже параллельно!

# Сухая установка (посмотреть что будет установлено)
uv pip install --dry-run requests
```

## 📊 Сравнение производительности

```bash
# Тест скорости (на проекте с 100+ зависимостями)
time pip install -r requirements.txt   # ~15-20 секунд
time uv pip install -r requirements.txt # ~1-2 секунды (10x быстрее!)

# Установка одного пакета
time pip install requests   # ~1.5 секунды
time uv pip install requests # ~0.2 секунды
```

## ⚠️ Частые проблемы и решения

```bash
# Проблема: "No matching distribution found"
# Решение: Проверить версию Python и платформу
uv pip install --python-version 3.11 --platform linux_x86_64 package

# Проблема: Конфликты зависимостей
# Решение: Использовать uv sync (не uv pip) для полного разрешения
uv sync  # Лучше для проектов с pyproject.toml

# Проблема: Медленная загрузка
# Решение: Использовать ближайший индекс
uv pip install --index-url https://mirror.yandex.ru/pypi/simple/ package

# Проблема: Сломанный кэш
uv cache clean
uv pip install --no-cache package
```

## 🚨 Важные отличия от pip

1. **Скорость**: uv в 10-100 раз быстрее
2. **Параллельность**: Все операции автоматически параллельны
3. **Кэширование**: Более агрессивное и эффективное
4. **Разрешение зависимостей**: Использует современный SAT-солвер
5. **Совместимость**: 100% обратная совместимость с pip CLI

## 🔗 Полезные ссылки

- [Официальная документация](https://docs.astral.sh/uv/)
- [GitHub репозиторий](https://github.com/astral-sh/uv)
- [Сравнение с другими инструментами](https://docs.astral.sh/uv/#comparison)

**Главный совет:** Используйте `uv` везде, где обычно использовали `pip` — он работает так же, но намного быстрее!