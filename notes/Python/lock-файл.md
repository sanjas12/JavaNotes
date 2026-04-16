## Что такое lock-файл?

**Lock-файл** (файл блокировки зависимостей) — это файл, который фиксирует **точные версии** всех пакетов и их транзитивных зависимостей, включая хэши для верификации.

В отличие от `requirements.txt`, который может содержать гибкие версии (`requests>=2.28.0`), lock-файл замораживает **каждую конкретную версию** каждого пакета.

## 📦 Сравнение: requirements.txt vs lock-файл

### `requirements.txt` (не lock)
```txt
# Может быть неопределённость
requests>=2.28.0
pandas
numpy==1.24.0
```

### Lock-файл (точные версии)
```txt
# lock-файл (uv.lock)
[
  {
    "name": "requests",
    "version": "2.31.0",
    "source": "https://pypi.org/simple",
    "dependencies": [
      "charset-normalizer<4,>=2",
      "idna<4,>=2.5",
      "urllib3<3,>=1.21.1",
      "certifi>=2017.4.17"
    ]
  },
  {
    "name": "charset-normalizer",
    "version": "3.3.2",
    "source": "https://pypi.org/simple"
  }
  # ... все транзитивные зависимости
]
```

## 🎯 Зачем нужен lock-файл?

### 1. **Воспроизводимость**
```bash
# Без lock — непредсказуемо
pip install -r requirements.txt
# Сегодня установится requests 2.31.0
# Завтра — requests 2.32.0 (может сломать код)

# С lock — детерминированно
uv pip sync uv.lock
# Всегда одинаковые версии
```

### 2. **Безопасность**
```bash
# Lock-файл с хэшами
uv pip freeze --hash
# requests==2.31.0 \
#   --hash=sha256:942c5a758f98d790eaed1a29cb6eefc7ffb0d1cf7af05c3d2791656dbd6ad1e1
```

### 3. **Командная разработка**
- Все разработчики используют **одинаковые** версии
- Нет конфликтов "у меня работает, а у тебя нет"

### 4. **CI/CD и деплой**
```yaml
# GitHub Actions
- name: Install dependencies
  run: |
    uv sync --frozen  # Использовать только lock-файл
    # Не обновлять зависимости автоматически
```

## 🔧 Lock-файлы в разных инструментах

| Инструмент | Имя lock-файла | Команда создания |
|------------|----------------|------------------|
| **uv** | `uv.lock` | `uv lock` |
| **Poetry** | `poetry.lock` | `poetry lock` |
| **Pipenv** | `Pipfile.lock` | `pipenv lock` |
| **PDM** | `pdm.lock` | `pdm lock` |
| **Conda** | `conda-lock.yml` | `conda-lock lock` |

## 🚀 Работа с lock-файлами в uv

### Создание lock-файла
```bash
# Из pyproject.toml
uv lock

# Из requirements.txt
uv pip compile requirements.txt -o uv.lock
```

### Синхронизация (установка из lock)
```bash
# Установить точно из lock-файла
uv sync

# Замороженный режим (не обновлять lock)
uv sync --frozen

# Только проверить, не устанавливая
uv sync --dry-run
```

### Обновление lock-файла
```bash
# Обновить все пакеты до последних версий
uv lock --upgrade

# Обновить конкретный пакет
uv lock --upgrade-package requests

# Обновить только определённые
uv lock --upgrade-package requests --upgrade-package pandas
```

## 📝 Пример полного workflow

### `pyproject.toml` (гибкие требования)
```toml
[project]
name = "myapp"
version = "0.1.0"
dependencies = [
    "fastapi>=0.100.0",
    "uvicorn>=0.23.0",
    "requests>=2.28.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=23.0.0",
]
```

### Создание lock-файлов
```bash
# Создаём lock для основных зависимостей
uv lock

# С lock для dev зависимостей
uv lock --extra dev

# Результат: uv.lock (фиксирует все версии)
```

### Использование в разных средах
```bash
# Разработка (с dev зависимостями)
uv sync --extra dev

# Production (только основные)
uv sync --no-dev

# CI (строго из lock)
uv sync --frozen --no-dev
```

## ✅ Преимущества lock-файлов

1. **Детерминизм**: Одинаковые версии везде
2. **Безопасность**: Хэши предотвращают подмену пакетов
3. **Скорость**: Не нужно разрешать зависимости каждый раз
4. **Офлайн-установка**: Можно скачать точно указанные версии
5. **Аудит**: Легко отследить, что именно установлено

## ⚠️ Недостатки

1. **Дополнительный файл** в репозитории
2. **Конфликты при слиянии** (merge conflicts)
3. **Ручное обновление** для получения новых версий
4. **Большой размер** (может быть тысячи строк)

## 🔄 Когда обновлять lock-файл?

```bash
# ✅ НУЖНО обновлять lock, когда:
# - Добавляете новый пакет
# - Удаляете пакет
# - Обновляете версии в pyproject.toml
# - Исправляете уязвимость

# ❌ НЕ НУЖНО обновлять lock, когда:
# - Меняете только код приложения
# - Обновляете документацию
# - Меняете конфигурацию CI
```

## 🛠️ Практические команды

### uv специфичные команды
```bash
# Проверить, синхронизирован ли lock с pyproject.toml
uv lock --check

# Показать различия между lock и текущим окружением
uv sync --dry-run

# Экспорт lock в requirements.txt
uv export --format requirements-txt > requirements.txt

# Экспорт с хэшами
uv export --format requirements-txt --hash > requirements-locked.txt
```

### Решение конфликтов lock-файла
```bash
# При merge конфликтах в uv.lock
# 1. Удалить конфликтный файл
rm uv.lock

# 2. Пересоздать заново
uv lock

# 3. Проверить, что всё работает
uv sync --frozen
```

## 📊 Реальный пример

### Без lock-файла:
```bash
# Сегодня
pip install fastapi  # Установится fastapi 0.104.0

# Через месяц
pip install fastapi  # Установится fastapi 0.115.0 (может сломать API)
```

### С lock-файлом:
```bash
# uv.lock фиксирует fastapi==0.104.0
uv sync  # Всегда установит 0.104.0

# Чтобы обновить:
uv lock --upgrade-package fastapi  # Обновит до 0.115.0
uv sync  # Теперь новая версия
```

## 🎯 Итог

**Lock-файл** — это "снимок" всех зависимостей проекта в конкретный момент времени. Он обеспечивает:

- 🔒 **Воспроизводимость** — одинаковые версии везде
- 🚀 **Надёжность** — никаких сюрпризов при установке  
- 🤝 **Командная работа** — единое окружение у всех
- 🔐 **Безопасность** — верификация через хэши

**В uv lock-файл создаётся автоматически через `uv lock` и используется через `uv sync`.**

Для большинства проектов lock-файл **обязателен** к хранению в репозитории!