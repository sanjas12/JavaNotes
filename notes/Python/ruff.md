`ruff` — это **экстремально быстрый** линтер и автоформаттер для Python, написанный на Rust. Он заменяет сразу несколько инструментов: `flake8`, `isort`, `black`, `pydocstyle`, `pyupgrade` и другие.

## Установка

```bash
# Через pip
pip install ruff

# Через uv (быстрее)
uv tool install ruff

uv tool install ruff@latest

# Через pipx (изолированно)
pipx install ruff

# В проекте (как зависимость)
uv add --dev ruff
# или
pip install ruff
```

## Основные команды

### 1. **Линтинг** (проверка кода)
```bash
# Проверить файл/папку
ruff check .

# Проверить конкретный файл
ruff check main.py

# Только ошибки (без предупреждений)
ruff check --select E .

# Исправить автоматически (что можно)
ruff check --fix .

# Показать детали правил
ruff check --explain E501  # Объяснение правила E501 (длинная строка)
```

### 2. **Форматирование** (как Black)
```bash
# Отформатировать код
ruff format .

# Проверить формат (без изменений)
ruff format --check .

# Форматирование с диффом
ruff format --diff .
```

### 3. **Оба действия вместе**
```bash
# Сначала исправить, потом отформатировать
ruff check --fix . && ruff format .
```

## Конфигурация

### `pyproject.toml` (рекомендуемый способ)
```toml
[tool.ruff]
# Выбор правил (какие проверки включить)
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # Pyflakes
    "I",   # isort
    "N",   # pep8-naming
    "UP",  # pyupgrade
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "SIM", # flake8-simplify
    "T20", # flake8-print (запрет print)
]

# Игнорировать некоторые правила
ignore = [
    "E501",  # длинные строки (обычно не критично)
    "E203",  # конфликт с black
]

# Исключить папки
exclude = [
    ".git",
    "__pycache__",
    "venv",
    "migrations",
]

# Максимальная длина строки (для форматирования)
line-length = 88

[tool.ruff.lint.per-file-ignores]
# Особые правила для тестов
"tests/**/*.py" = ["S101"]  # разрешить assert в тестах

[tool.ruff.format]
# Настройки форматирования (как black)
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
```

### `.ruff.toml` (альтернативный файл)
```toml
# Можно использовать отдельный файл вместо pyproject.toml
target-version = "py312"
line-length = 100

select = ["E", "F", "I"]
ignore = ["E501"]
```

## Примеры использования

### Базовый пример
```python
# before.py
import os,sys
def hello():
  print("hello")
  x=1
  return x

def unused():  # предупреждение: неиспользуемая функция
    pass
```

```bash
ruff check before.py
# Вывод:
# before.py:1:8: F401 [*] `os` imported but unused
# before.py:1:12: E231 [*] Missing whitespace after ','
# before.py:1:12: F401 `sys` imported but unused
# before.py:2:1: E302 Expected 2 blank lines, found 1
# before.py:3:3: E111 Indentation is 2 spaces (expected 4)
# before.py:4:3: E225 Missing whitespace around operator

ruff check --fix before.py  # Автоисправление
ruff format before.py       # Форматирование
```

### Сложные сценарии
```bash
# Только определённые категории ошибок
ruff check --select F,E501 .

# Все правила (включая экспериментальные)
ruff check --select ALL .

# Проверить только новые изменения (с Git)
ruff check --statistics --fix .

# В CI (строгий режим)
ruff check --format=github --output-format=sarif .

# С кэшированием (быстрее)
ruff check --cache-dir .ruff_cache .
```

## Интеграция с редакторами

### VS Code
```json
{
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": true,
      "source.organizeImports.ruff": true
    }
  },
  "ruff.enable": true
}
```

### PyCharm
- Установить плагин Ruff
- Настроить External Tools:
  - `ruff check --fix $FilePath$`
  - `ruff format $FilePath$`

### Pre-commit hook
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format
```

## Полезные команды

```bash
# Показать все доступные правила
ruff rule --all

# Объяснение конкретного правила
ruff rule E501

# Проверка с выводом в JSON (для CI)
ruff check --output-format=json .

# Статистика по типам ошибок
ruff check --statistics .

# Только добавить импорты (I) и обновить синтаксис (UP)
ruff check --select I,UP --fix .

# Проверить, но не показывать фиксы
ruff check --no-fix .

# Показать, что будет изменено при форматировании
ruff format --diff .
```

## Ruff vs Альтернативы (скорость)

| Инструмент | Время на 100к строк | Количество правил |
|------------|---------------------|-------------------|
| **Ruff**   | ~0.5 сек            | 700+              |
| Flake8     | ~15 сек             | ~150              |
| Pylint     | ~60 сек             | ~300              |
| Black      | ~5 сек              | только формат      |

## Пример полного рабочего процесса

```bash
# Создать проект с ruff
mkdir myproject && cd myproject
python -m venv venv
source venv/bin/activate
pip install ruff

# Создать конфиг
cat > pyproject.toml << EOF
[tool.ruff]
select = ["E", "F", "I", "N", "UP", "B", "SIM"]
line-length = 88
target-version = "py312"

[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]
EOF

# Проверить код
ruff check --fix .
ruff format .

# Добавить pre-commit
pip install pre-commit
cat > .pre-commit-config.yaml << EOF
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
EOF

pre-commit install
```

## Советы

1. **Начинайте с `select = ["E", "F", "I"]`** — самые важные правила
2. **Используйте `ruff check --fix` перед коммитом**
3. **В CI используйте `ruff check --output-format=sarif`** для интеграции с GitHub Security
4. **Ruff может заменить до 10 инструментов** — упростите конфигурацию
5. **Для старых проектов** добавляйте правила постепенно через `--select`

Ruff действительно очень быстрый — вы не заметите задержки даже на огромных проектах с тысячами файлов!