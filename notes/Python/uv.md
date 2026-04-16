Отличный выбор! `uv` — это новый, чрезвычайно быстрый менеджер пакетов и инструмент для управления Python-окружениями, написанный на Rust (создатель `ruff`). Он призван заменить `pip`, `pip-tools`, `virtualenv`, `poetry` и отчасти `pyenv`.

Вот **основы работы с `uv`** для повседневных задач.

## 1. Установка

**macOS / Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows (PowerShell):**
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Через pip (не рекомендуется, но можно):**
```bash
pip install uv

pip install uv --upgrade # обновление
```

После установки проверьте: `uv --version`.

## 2. Создание виртуального окружения

В отличие от `python -m venv`, `uv` делает это мгновенно.

```bash
# Создать окружение в папке .venv
uv venv

# Создать с конкретной версией Python
uv venv --python 3.12

# С указанием имени папки
uv venv my_env
```

**Активация** (стандартно, как и для venv):
- **Linux/macOS:** `source .venv/bin/activate`
- **Windows:** `.venv\Scripts\activate`

Деактивация окружения:
	`deactivate`


Но `uv` может работать и без явной активации (см. раздел "Запуск скриптов").

## 3. Установка пакетов

### Добавление пакетов в текущее окружение:
```bash
uv pip install requests
uv pip install numpy pandas
```

### Установка из файла зависимостей:
```bash
# Поддерживает requirements.txt
uv pip install -r requirements.txt

# Поддерживает pyproject.toml (как poetry)
uv pip install -e .
```

### Установка с опциями (dev, extras):
```bash
uv pip install "package[extra]"
uv pip install pytest --dev  # условно, но обычно через группы
```

## 4. Управление зависимостями (как poetry/pip-tools)

Это killer-фича `uv` — детерминированные зависимости с lock-файлом.

### Инициализация проекта:
```bash
uv init my_project
cd my_project
```
Это создаст `pyproject.toml` и `README.md`.

### Добавление зависимости:
```bash
uv add requests
```
Что произойдет:
1. Добавит `requests` в `pyproject.toml`
2. Создаст/обновит `uv.lock` (фиксированные версии всех зависимостей)
3. (Опционально) установит в текущее окружение

### Добавление dev-зависимости:
```bash
uv add --dev pytest black
```

### Удаление:
```bash
uv remove requests
```

### Синхронизация окружения с lock-файлом:
```bash
uv sync
```
Эта команда идеальна для CI/CD — она приводит `.venv` в точное соответствие с `uv.lock`.

## 5. Запуск скриптов (без активации!)

Это очень удобно — `uv` сам найдет нужное окружение.

```bash
# Запуск Python-скрипта с использованием окружения из .venv
uv run python script.py

# Запуск установленной консольной утилиты
uv run pytest
uv run black .

# Запуск одного файла с автоматической установкой зависимостей (экспериментально)
uv run --script script.py  # если в начале файла есть комментарий с зависимостями
```

## 6. Работа с разными версиями Python

`uv` умеет скачивать и использовать разные версии Python.

```bash
# Установить конкретную версию Python
uv python install 3.11 3.12

# Посмотреть доступные версии
uv python list

# Создать окружение с нужной версией
uv venv --python 3.11
```

## 7. Полезные команды

```bash
# Показать дерево зависимостей
uv pip tree

# Проверить, нет ли конфликтов в зависимостях
uv pip check

# Обновить пакет до последней версии
uv pip install --upgrade requests

# Заморозить текущие пакеты в requirements.txt
uv pip freeze > requirements.txt

# Экспорт lock-файла в requirements.txt (для совместимости)
uv export --format requirements-txt > requirements.txt
```

## 8. Пример рабочего процесса

```bash
# 1. Создать новый проект
uv init my_app
cd my_app

# 2. Добавить зависимости
uv add fastapi uvicorn
uv add --dev pytest httpx

# 3. Создать и синхронизировать окружение
uv venv
uv sync

# 4. Написать код в main.py
# 5. Запустить без активации
uv run uvicorn main:app --reload

# 6. Запустить тесты
uv run pytest
```

## 9. Главные отличия от других инструментов

| Инструмент | Чем `uv` лучше |
|------------|----------------|
| **pip + venv** | В 10-100 раз быстрее, автоматический lock-файл |
| **poetry** | Намного быстрее, нет проблем с несовместимостью плагинов |
| **pip-tools** | Встроенная функциональность, один инструмент |
| **conda** | Легче, быстрее, не тянет лишних зависимостей |

## 10. Когда НЕ использовать `uv`?
- Если нужны **conda-пакеты** с бинарными зависимостями (например, GDAL, OpenCV с CUDA)
- Если проект жестко привязан к `poetry` или `pipenv` в CI
- На очень старых системах (хотя `uv` самодостаточен)

## Полезные ссылки:
- [Официальная документация](https://docs.astral.sh/uv/)
- [GitHub репозиторий](https://github.com/astral-sh/uv)

**Совет:** Начните использовать `uv` в новом проекте — вы сразу почувствуете разницу в скорости. Для старых проектов начните с `uv pip` как замены `pip`, а потом переходите на `uv add` + `uv.lock`.