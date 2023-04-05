
[![Django-app workflow](https://github.com/sreutov2008/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/sreutov2008/yamdb_final/actions/workflows/yamdb_workflow.yml)


# Проект YaMDb
Проект YaMDb собирает отзывы пользователей на произведения.
Произведения делятся на категории, им  может быть присвоен жанр из списка предустановленных. 
Пользователи могут оставлить к произведениям текстовые отзывы и ставить произведению оценку, оставлять комментарии к отзывам.

### Содержание:
 - [Установка проекта из репозитория](#установка-проекта)
 - [Реализованы возможности](#реализованы-возможности)
 - [Примеры запросов](#примеры-запросов)
 - [Автор](#автор)

## Особенности реализации

- Проект завернут в Docker-контейнеры;
- Реализован workflow через GitHubActions: тестирование, обновление образа на DockerHub,
  автоматический деплой на сервер, отправление сообщения в Telegram об успешном выполнении всех шагов workflow;
- Проект развернут на сервере <http://158.160.15.96/>


### Установка проекта из репозитория

1. Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/Sreutov2008/yamdb_final.git
```
```
cd yamdb_final
```
2. Создать и открыть файл ```.env ``` с переменными окружения

```
cd infra
touch .env
```
3. Заполнить ```.env ``` файл по примеру:

```
DB_ENGINE=django.db.backends.postgresql 
DB_NAME= имя базы данных postgres
POSTGRES_USER=  логин для подключения к базе данных
POSTGRES_PASSWORD= пароль для подключения к БД (установите свой)
DB_HOST= db
DB_PORT=5432
SECRET_KEY = секретный ключ проекта django
```

4. Установка и запуск приложения в контейнерах:
``` 
docker-compose up -d --build
```

5. Запуск миграций, создание суперюзера, сбор статики и заполнение БД:
``` 
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
docker-compose exec web python manage.py loaddata fixtures.json
```


### Реализованы возможности
* Получение, создание, удаление категорий произведения.
* Получение, создание, удаление жанров произведения.
* Получение, создание, обновление, удаление произведений.
* Получение, создание, обновление, удаление отзывов.
* Получение, создание, обновление, удаление комментариев к отзыву.
* Получение, создание, обновление, удаление пользователей.
* Регистрация пользователей и выдача токенов





### Примеры запросов

#### Публикация и получение категорий

Request: [GET] http://127.0.0.1:8000/api/v1/categories/

Response:
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "name": "string",
      "slug": "string"
    }
  ]
}
```
Request: [POST] http://127.0.0.1:8000/api/v1/categories/

Response:
```
{
  "name": "string",
  "slug": "string"
}
```

#### Частичное обновление и получение произведений
Request: [GET] http://127.0.0.1:8000/api/v1/titles/

Response:
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "name": "string",
      "year": 0,
      "rating": 0,
      "description": "string",
      "genre": [
        {
          "name": "string",
          "slug": "string"
        }
      ],
      "category": {
        "name": "string",
        "slug": "string"
      }
    }
  ]
}
```
Request: [PATCH] http://127.0.0.1:8000/api/v1/titles/{titles_id}/

Response:
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

#### Публикация и удаление комментария

Request: [POST] http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/

Response:
```
{
  "text": "string"
}
```
Request: [DELETE] http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/



### Автор
 * Реутов Александр (Sreutov2008@yandex.ru, https://github.com/Sreutov2008)