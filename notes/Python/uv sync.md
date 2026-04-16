Отличная команда! `uv sync` — это одна из главных команд в `uv`, которая синхронизирует ваше виртуальное окружение с файлами проекта. Давайте разберем, как она работает.

## Что делает `uv sync`

`uv sync` **приводит ваше виртуальное окружение в точное соответствие** с `pyproject.toml` и `uv.lock`:

1. ✅ **Устанавливает** недостающие пакеты
2. ✅ **Обновляет** пакеты до версий из [[lock-файл]]
3. ✅ **Удаляет** лишние пакеты (которых нет в зависимостях)
4. ✅ **Создает** виртуальное окружение, если его нет

## Основное использование

```bash
# Базовая синхронизация
uv sync

# С dev-зависимостями
uv sync --dev

# Без установки dev-зависимостей
uv sync --no-dev

# Только установка, без обновления lock-файла
uv sync --frozen

# С обновлением всех зависимостей
uv sync --upgrade

# С обновлением конкретных пакетов
uv sync --upgrade-package requests --upgrade-package pytest
```

## Пример рабочего процесса

```bash
# 1. Создаем проект
uv init my_project
cd my_project

# 2. Добавляем зависимости
uv add fastapi uvicorn
uv add --dev pytest black

# 3. Синхронизируем окружение
uv sync --dev

# Результат:
# - Создана папка .venv
# - Установлены fastapi, uvicorn, pytest, black
# - Создан uv.lock с точными версиями
```

## Что происходит под капотом

```bash
uv sync --dev
```

**Шаги:**
1. Проверяет наличие `.venv` (создает, если нет)
2. Читает `pyproject.toml` и `uv.lock` (создает lock, если нет)
3. Вычисляет разницу между желаемым и текущим состоянием
4. Устанавливает/обновляет/удаляет пакеты
5. Синхронизирует окружение

## Часто используемые флаги

| Флаг | Описание |
|------|----------|
| `--dev` | Включить dev-зависимости (`tool.uv.dev-dependencies`) |
| `--no-dev` | Исключить dev-зависимости |
| `--frozen` | Использовать существующий lock-файл без обновления |
| `--upgrade` | Обновить все пакеты до последних версий |
| `--upgrade-package` | Обновить только указанные пакеты |
| `--link-mode` | Режим ссылок (`hardlink`, `copy`, `symlink`) |
| `--verbose` | Подробный вывод для отладки |
| `--dry-run` | Показать, что будет сделано, без выполнения |

## Практические сценарии

### 1. **Первый запуск проекта**
```bash
git clone https://github.com/user/project.git
cd project
uv sync --dev  # Установит всё необходимое
```

### 2. **После добавления новых зависимостей**
```bash
uv add requests  # Добавил в pyproject.toml
uv sync          # Установит requests в .venv
```

### 3. **Обновление зависимостей**
```bash
# Обновить всё
uv sync --upgrade

# Обновить только конкретный пакет
uv sync --upgrade-package fastapi

# Обновить с сохранением lock-файла
uv lock --upgrade
uv sync --frozen
```

### 4. **CI/CD пайплайн**
```bash
# В production окружении (без dev)
uv sync --frozen --no-dev

# В тестовом окружении (с dev)
uv sync --frozen --dev
```

### 5. **Очистка окружения**
```bash
# Удалить пакеты, которых нет в зависимостях
uv sync --no-dev  # Удалит dev-пакеты

# Полная переустановка
rm -rf .venv
uv sync --dev
```

## Сравнение с другими инструментами

| Инструмент | Аналог `uv sync` |
|------------|------------------|
| **poetry** | `poetry install` |
| **pipenv** | `pipenv install` |
| **pip + venv** | `pip install -r requirements.txt` (но без удаления лишних) |
| **conda** | `conda env update` |

## Продвинутое использование

### С несколькими группами зависимостей
```toml
# pyproject.toml
[project.optional-dependencies]
test = ["pytest", "httpx"]
docs = ["mkdocs", "sphinx"]

[tool.uv]
dev-dependencies = ["black", "ruff"]
```

```bash
# Только основные
uv sync

# Основные + dev
uv sync --dev

# Основные + test (через pip install)
uv sync && uv pip install -e ".[test]"
```

### Использование с environment markers
```bash
# Синхронизация для конкретной платформы
uv sync --python-platform linux

# Синхронизация с конкретной версией Python
uv sync --python 3.11
```

## Частые проблемы и их решение

### Проблема 1: Lock файл устарел
```bash
# Ошибка: lock file is outdated
# Решение:
uv lock
uv sync
```

### Проблема 2: Конфликт версий Python
```bash
# Ошибка: Python version mismatch
# Решение:
uv venv --python 3.11
uv sync
```

### Проблема 3: Медленная работа
```bash
# Использовать кэширование
export UV_LINK_MODE=hardlink  # или copy
uv sync --frozen  # быстрее без проверок
```

## Лучшие практики

### 1. **Всегда коммитьте uv.lock**
```bash
git add uv.lock
git commit -m "Update dependencies"
```

### 2. **Используйте --frozen в CI**
```yaml
# .github/workflows/ci.yml
- name: Install dependencies
  run: uv sync --frozen --dev
```

### 3. **Разделяйте dev и prod зависимости**
```bash
# Разработка
uv sync --dev

# Production
uv sync --no-dev
```

### 4. **Проверяйте перед коммитом**
```bash
uv sync --dry-run  # Посмотреть, что изменится
uv lock --check    # Проверить lock файл
uv pip check       # Проверить конфликты
```

## Полезные алиасы

```bash
# Добавьте в ~/.bashrc
alias us='uv sync'
alias usd='uv sync --dev'
alias usf='uv sync --frozen'
alias usu='uv sync --upgrade'

# Использование
usd  # синхронизация с dev-зависимостями
```

## Итог

`uv sync` — это **главная команда для управления окружением**:

- ✅ **Всегда используйте** после `git clone`
- ✅ **Всегда используйте** после `uv add` или `uv remove`
- ✅ **В CI** используйте `uv sync --frozen --no-dev`
- ✅ **При разработке** используйте `uv sync --dev`

```bash
# Стандартный рабочий процесс
uv add package          # добавить зависимость
uv sync                 # синхронизировать окружение
uv run python script.py # запустить скрипт
```