### Общие сведения
Коллекция [Postman](https://www.getpostman.com/collections/914bdb90302845426060)  

Репозиторий содержит api-сервис, написанный на FastAPi.
Базы данных:
- PostgreSQL в качестве хранилища постов и данных пользователей
- Redis как кэш JWT токенов

Во время работы сервиса возникает единовременный не критический SAWarning из-за  
использования execute метода sqlmodel.

### Запуск:  
Зависимости:
- Docker
- Docker-compose

1. Клонировать репозиторий и перейти в корневой каталог задания
```bash
git clone https://github.com/Wayfarer545/FastAPI_JWT && cd FastAPI_JWT
```
2. Для использования сервиса необходимо обозначить каталог монтирования базы данных Postgres  
в файле docker-compose:
```yaml
  ylab_postgres_db:
    container_name: my_postgres_db
    image: postgres:latest
    volumes:
      - /path/to/db:/var/lib/postgresql/data/
```
3. Инициализировать сборку приложения и запуск зависимостей
```bash
docker-compose up
```

В случае использования скрипта вне контейнера Docker на базе ОС Windows, необходимо  
внести изменения в файл alembic.ini секции script_location = src/migrations, 
заменив на src\migrations.  

---


Основные изменения:
- добавлен роутер, касающийся авторизованных действий
- добавлены схемы типизации Request/Response 
- добавлен класс, отвечающий за логику эндпоинтов авторизованных действий
- обновлён класс RedisCache
- добавлены два соединения Redis (правильнее было бы через пул заводить)
- добавлена миграция Account, отвечающая за таблицу с данными пользователей
- Кэш в Redis содержит black list и перечень всех refresh tokens с доступом по uuid
- обновлён файл requirements.txt

Изменения в коллекции Postman:
- добавлен скрипт присвоения нового access token при  
обновлении информации о себе;
- запросы типа login передают только username и password  
- запросы типа logout передаются с access token

