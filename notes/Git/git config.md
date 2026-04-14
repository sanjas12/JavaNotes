Вот все способы посмотреть конфиги Git — от глобальных до локальных и системных:

## 1. **Просмотр всех конфигов (с приоритетом)**
```bash
git config --list --show-origin
```
Показывает **все** настройки + откуда каждая взята (файл и строка). Самый полный вариант.

## 2. **По уровням конфигурации**

### Глобальный (для текущего пользователя)
```bash
git config --global --list
# или с путём к файлу:
git config --global --list --show-origin
# файл: ~/.gitconfig или ~/.config/git/config
```

### Локальный (для текущего репозитория)
```bash
git config --local --list
# файл: .git/config в текущем репозитории
```

### Системный (для всех пользователей ОС)
```bash
git config --system --list
# файл: /etc/gitconfig
```

## 3. **Посмотреть конкретную настройку**
```bash
git config user.name
git config --global core.editor
git config pull.rebase
```

## 4. **Посмотреть где находятся конфиг-файлы**
```bash
git config --list --show-origin | head -5   # первые 5 записей
# или
git config --global --show-origin --list | head -1
```

## 5. **Просмотр с сортировкой и фильтрацией**

### Отфильтровать по ключу
```bash
git config --list | grep user
git config --list | grep alias
git config --list | grep pull
```

### Увидеть только уникальные (без дубликатов)
```bash
git config --list --global | sort -u
```

## 6. **Просмотр содержимого конкретного файла**

```bash
# Глобальный конфиг
cat ~/.gitconfig
# или
cat ~/.config/git/config

# Локальный (внутри репозитория)
cat .git/config

# Системный
cat /etc/gitconfig
```

## 7. **Проверка приоритета настроек** (важно!)

Git читает конфиги в порядке:
1. Системный (самый низкий приоритет)
2. Глобальный
3. Локальный (самый высокий приоритет)

Проверить какое значение реально используется:
```bash
git config user.name   # покажет итоговое значение
```

## 8. **Удобный скрипт для просмотра всех уровней**

```bash
echo "=== SYSTEM CONFIG ===" && git config --system --list 2>/dev/null
echo "=== GLOBAL CONFIG ===" && git config --global --list
echo "=== LOCAL CONFIG ===" && git config --local --list 2>/dev/null
```

## 9. **Самые частые команды для проверки**

| Что посмотреть | Команда |
|----------------|---------|
| Все настройки с источниками | `git config --list --show-origin` |
| Только глобальные | `git config --global --list` |
| Алиасы | `git config --list \| grep alias` |
| Настройки pull/push | `git config --list \| grep pull` |
| Кто автор в текущем репозитории | `git config user.name && git config user.email` |

## 10. **Для вашей системы (Astra Linux)**

Так как вы работаете в Astra Linux, пути стандартные:
```bash
# Посмотреть все глобальные настройки
git config --global --list

# Проверить что настроено для pull (выше обсуждали про лучшие практики)
git config --list | grep pull
```

## Полезный алиас для быстрого просмотра

```bash
git config --global alias.configs '!echo "=== GLOBAL ===" && git config --global --list && echo "=== LOCAL ===" && git config --local --list'
```

После этого можно просто:
```bash
git configs
```