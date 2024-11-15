---
tags: Микросервисы
---
## Основные команды
1. do**docker pull**: Скачать Docker образ из репозитория. Пример: `docker pull nginx` (скачать образ Nginx).
2. **docker build**: Создать Docker образ из Dockerfile. Пример: `docker build -t my-image-name /path/to/dockerfile` (создать образ с именем my-image-name из указанного Dockerfile).
3. **docker run**: Запустить новый контейнер из образа. Пример: `docker run -d -p 8080:80 --name my-container-name nginx` (запустить контейнер на порту 8080, назвать его my-container-name). [[Структура команды docker run]]
4. **docker ps**: Показать список запущенных контейнеров. Пример: `docker ps` (показать текущие контейнеры).
5. **docker images**: Показать список скачанных Docker образов. Пример: `docker images` (показать доступные образы).
6. **docker exec**: Выполнить команду внутри запущенного контейнера. Пример: `docker exec -it my-container-name sh` (войти в контейнер с интерактивным доступом к оболочке).
7. **docker stop**: Остановить запущенный контейнер. Пример: `docker stop my-container-name` (остановить контейнер с именем my-container-name).
8. **docker rm**: Удалить контейнер. Пример: `docker rm my-container-name` (удалить контейнер с именем my-container-name).
9. **docker rmi**: Удалить Docker образ. Пример: `docker rmi nginx` (удалить образ Nginx).
10. **docker-compose up**: Запустить сервисы, описанные в файле `docker-compose.yml`. Пример: `docker-compose up -d` (запустить в фоновом режиме).
11. **docker-compose down**: Остановить и удалить сервисы, описанные в файле `docker-compose.yml`. Пример: `docker-compose down` (остановить и удалить контейнеры).
12. **docker logs**: Просмотреть логи контейнера. Пример: `docker logs my-container-name` (показать логи контейнера с именем my-container-name).
13. **docker network**: Управление сетями Docker. Пример: `docker network create my-network` (создать сеть с именем my-network).
14. **docker volume**: Управление Docker томами (механизм для сохранения данных между запусками контейнеров). Пример: `docker volume create my-volume` (создать том с именем my-volume).
15. **docker inspect**: Получить информацию о Docker объекте (контейнер, образ и т.д.). Пример: `docker inspect my-container-name` (получить информацию о контейнере с именем my-container-name).


Это только небольшой набор команд Docker. Для более подробной информации вы можете использовать официальную документацию Docker или выполнить `docker --help` для получения списка доступных команд.
[[Фоновый и интерактивный режим запуска контейнеров]]

## Опции командной строки Docker
Используются для настройки и управления поведением контейнеров при их запуске. Вот некоторые часто используемые опции:

1. **-p, --publish**: Проброс портов. Пример: `docker run -p <host_port>:<container_port> ...` Эта опция позволяет пробросить порт контейнера на порт хостовой машины.
2. **-d, --detach**: Запустить контейнер в фоновом режиме. Пример: `docker run -d ...` Контейнер будет запущен в фоновом режиме, и возвращен будет только его ID.
3. **--name**: Задать имя контейнера. Пример: `docker run --name my-container ...` Эта опция позволяет задать имя для контейнера вместо автоматически сгенерированного имени.
4. **-v, --volume**: Привязка томов (монтирование директорий). Пример: `docker run -v <host_path>:<container_path> ...` Эта опция позволяет монтировать директории с хостовой машины внутрь контейнера.
5. **--network**: Присоединить контейнер к сети. Пример: `docker run --network my-network ...` Эта опция определяет, к какой сети должен присоединиться контейнер.
6. **-e, --env**: Установка переменных окружения. Пример: `docker run -e VAR=value ...` Эта опция позволяет задавать переменные окружения внутри контейнера.
7. **--restart**: Поведение при автоматическом перезапуске контейнера. Пример: `docker run --restart=always ...` Эта опция указывает, когда контейнер должен быть автоматически перезапущен.
8. **-it**: Запуск контейнера в интерактивном режиме. Пример: `docker run -it ...` Эта опция позволяет подключиться к интерактивной оболочке контейнера.
9. **-v, --version**: Вывести версию Docker. Пример: `docker --version` Эта опция позволяет получить информацию о версии установленного Docker.

Это всего лишь небольшой набор опций. Docker предоставляет множество других опций для настройки контейнеров в соответствии с вашими потребностями. Вы можете ознакомиться со всеми доступными опциями для каждой команды, выполнив `docker команда --help` (например, `docker run --help`).
[[Запуск БД внутри контейнера]]
## Порты контейнеров в Docker
Это механизм, который позволяет устанавливать связь между приложением, работающим внутри контейнера, и хостовой операционной системой или внешней сетью. Контейнеры изолированы от внешнего окружения, поэтому чтобы позволить доступ к приложению внутри контейнера извне, необходимо настроить "проброс" (mapping) портов.

Когда вы запускаете контейнер, вы можете использовать параметр `-p` (или `--publish`), чтобы указать, какие порты приложения внутри контейнера должны быть доступны на хостовой машине или внешней сети. Синтаксис для проброса портов выглядит так:

`docker run -p <host_port>:<container_port> ...`

- `<host_port>` - это порт на хостовой машине, который будет привязан к порту внутри контейнера.
- `<container_port>` - это порт внутри контейнера, на котором работает ваше приложение.

Например, если вы запускаете веб-сервер внутри контейнера, который слушает порт 80, и вы хотите, чтобы этот веб-сервер был доступен на порту 8080 вашей хостовой машины, вы можете сделать следующее:

`docker run -p 8080:80 my-web-app-container`

>[!note]
>Это означает, что порт 80 внутри контейнера будет проброшен на порт 8080 на вашей хостовой машине. Теперь, если вы перейдете в браузере на `http://localhost:8080`, запрос будет перенаправлен на веб-сервер, работающий внутри контейнера.
>
>Проброс портов позволяет контейнерам взаимодействовать с внешним миром, а также обеспечивает изоляцию, так как внешние пользователи могут получить доступ только к тем портам, которые вы явно определили для проброса.

## Сети docker
Контейнеры в Docker могут общаться друг с другом с использованием сетей Docker. Каждый контейнер может быть добавлен в одну или несколько сетей, что позволяет им взаимодействовать между собой и с другими ресурсами сети, такими как хостовая машина или внешние сервисы.

Существует несколько типов сетей Docker:

1. [[Bridge Network Мостовая сеть Docker]]: Это наиболее распространенный тип сети по умолчанию. Контейнеры, добавленные в мостовую сеть, могут общаться друг с другом по их именам, а также по IP-адресам. Это обеспечивает локальную изоляцию между контейнерами и позволяет им легко обмениваться данными.

2. **Host Network (Сеть хоста)**: Контейнеры, добавленные в сеть хоста, используют сетевое пространство хостовой машины, а не изолированное сетевое пространство контейнера. Это означает, что контейнеры будут иметь доступ к тем же сетевым интерфейсам и портам, что и хостовая машина.

3. **Overlay Network (Накладная сеть)**: Этот тип сети предназначен для обеспечения коммуникации между контейнерами, размещенными на разных хостах в кластере Docker Swarm. Он позволяет контейнерам в разных хостах взаимодействовать, как будто они находятся в одной сети.

4. **Custom Bridge Network (Пользовательская мостовая сеть)**: Вы можете создать свои собственные пользовательские сети с помощью Docker CLI или Docker Compose. Это полезно, если вы хотите организовать специфическую инфраструктуру для ваших контейнеров.

Для того чтобы контейнеры могли взаимодействовать друг с другом, они должны быть добавлены в одну и ту же сеть. Как только они находятся в одной сети, вы можете использовать имена контейнеров для обращения к друг другу, либо использовать IP-адреса.

## Docker compose
Docker Compose - это инструмент для определения и запуска многоконтейнерных Docker приложений с помощью одного файла конфигурации. Он позволяет вам определить несколько контейнеров, их настройки, зависимости и параметры сети в одном файле, что упрощает развертывание и управление комплексными приложениями.

Чтобы настроить два контейнера с веб-приложением и базой данных PostgreSQL с использованием Docker Compose, вам нужно создать файл `docker-compose.yml` и определить в нем контейнеры и их настройки. Вот пример такого файла для вашей задачи:
```yaml
version: '3'
services:
  web-app:
    image: your-web-app-image:latest # Замените на имя вашего образа веб-приложения
    ports:
      - "80:80" # Проброс порта хоста на порт контейнера

  postgres-db:
    image: postgres:latest
    environment:
      POSTGRES_DB: your-database-name # Замените на имя вашей базы данных
      POSTGRES_USER: your-username    # Замените на имя пользователя
      POSTGRES_PASSWORD: your-password # Замените на пароль
    volumes:
      - postgres-data:/var/lib/postgresql/data # Монтирование тома для сохранения данных

volumes:
  postgres-data: # Определение тома для PostgreSQL
```
Замените комментарии на соответствующие значения для вашего приложения и базы данных. Вам также нужно будет подготовить Docker-образ для вашего веб-приложения и указать его имя в поле `image` для службы `web-app`.

После создания `docker-compose.yml` файла, выполните следующие шаги:

1. Убедитесь, что у вас установлен Docker и Docker Compose.
2. Откройте терминал и перейдите в каталог с `docker-compose.yml` файлом.
3. Выполните команду `docker-compose up -d` для запуска контейнеров в фоновом режиме.

Docker Compose загрузит и запустит контейнеры согласно вашему файлу конфигурации. Веб-приложение будет доступно по порту 80 хостовой машины. База данных PostgreSQL будет запущена в отдельном контейнере.

Не забудьте заменить имеющиеся комментарии на актуальные значения, связанные с вашим приложением и базой данных.
[[Что будет если один контейнер уже ранее был создан]]

### Docker образ (image) из Spring Boot приложения
1. **Создайте Dockerfile:**
Создайте файл с именем **Dockerfile** в корневой директории вашего проекта (там, где находится ваш код Spring Boot приложения). Вот пример базового **Dockerfile**:
```Dockerfile
## Используйте базовый образ с Java (выберите подходящий)
FROM openjdk:11

## Копируем JAR-файл с вашим приложением в контейнер
COPY target/your-app.jar /app.jar

## Устанавливаем рабочую директорию
WORKDIR /

## Команда, выполняющаяся при запуске контейнера
CMD ["java", "-jar", "app.jar"]

```
Замените target/your-app.jar на путь к JAR-файлу вашего собранного Spring Boot приложения.
2. **Соберите Docker образ (image):**
Откройте терминал, перейдите в директорию с Dockerfile и выполните следующую команду:
```bash
docker build -t your-image-name .
```
Здесь `your-image-name` - это имя, которое вы хотите присвоить вашему Docker образу. Точка `.` указывает Docker'у на текущую директорию, где находится
**Dockerfile**.
3. **Запустите контейнер из созданного образа:**
После успешной сборки образа вы можете запустить контейнер из этого образа:
```bash
docker run -p 8080:8080 your-image-name
```
1. Здесь `-p 8080:8080` пробрасывает порт 8080 из контейнера на порт 8080 хостовой машины. Исправьте порт, если ваше приложение слушает другой порт

Теперь ваше Spring Boot приложение должно быть доступно по адресу `http://localhost:8080`.