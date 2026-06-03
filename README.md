# Kittygram

Социальная сеть для публикации фотографий котиков. Пользователи могут регистрироваться, загружать фото своих питомцев и просматривать чужие публикации.

## Стек технологий

- **Backend:** Python 3.12, Django 5.1, Django REST Framework, Djoser
- **Frontend:** React
- **База данных:** PostgreSQL 13
- **Веб-сервер:** Nginx
- **Контейнеризация:** Docker, Docker Compose
- **CI/CD:** GitHub Actions

## Локальный запуск

**Требования:** Docker и Docker Compose

1. Клонировать репозиторий:
```bash
git clone https://github.com/Tusalvavidas14/kittygram_final.git
cd kittygram_final
```

2. Создать файл `.env` на основе `.env.example`:
```bash
cp .env.example .env
```

3. Запустить контейнеры:
```bash
docker compose up -d
```

4. Выполнить миграции и собрать статику:
```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic --noinput
docker compose exec backend cp -r /app/collected_static/. /backend_static/static/
```

Проект будет доступен по адресу: `http://localhost:9000`

## Переменные окружения

Все переменные описаны в `.env.example`:

| Переменная | Описание |
|---|---|
| `POSTGRES_DB` | Имя базы данных |
| `POSTGRES_USER` | Пользователь базы данных |
| `POSTGRES_PASSWORD` | Пароль базы данных |
| `DB_HOST` | Хост базы данных |
| `DB_PORT` | Порт базы данных |
| `DB_ENGINE` | Движок БД (по умолчанию PostgreSQL, если не задан — SQLite) |
| `SECRET_KEY` | Секретный ключ Django |
| `DEBUG` | Режим отладки (True/False) |
| `ALLOWED_HOSTS` | Разрешённые хосты через запятую |

## Деплой на сервер

CI/CD настроен через GitHub Actions. При пуше в ветку `main`:

1. Запускаются тесты (flake8 + Django tests + frontend tests)
2. Собираются и публикуются Docker-образы на DockerHub
3. Образы разворачиваются на сервере по SSH
4. В Telegram приходит уведомление об успешном деплое

**Необходимые GitHub Secrets:**

| Secret | Описание |
|---|---|
| `DOCKER_USERNAME` | Логин на DockerHub |
| `DOCKER_PASSWORD` | Пароль/токен DockerHub |
| `SERVER_HOST` | IP-адрес сервера |
| `SERVER_USER` | Пользователь на сервере |
| `SSH_PRIVATE_KEY` | Приватный SSH-ключ |
| `TELEGRAM_TO` | Telegram chat ID для уведомлений |
| `TELEGRAM_TOKEN` | Токен Telegram-бота |

## Docker-образы

Образы опубликованы на DockerHub:
- `glebusshek/kittygram_backend`
- `glebusshek/kittygram_frontend`
- `glebusshek/kittygram_gateway`
