# Kittygram

Kittygram — это веб-приложение, позволяющее пользователям создавать, просматривать и управлять профилями своих домашних питомцев. Оно разработано с использованием Django и предоставляет функциональность API для взаимодействия с данными.

## Основные возможности

- **Регистрация и аутентификация пользователей**
- **Создание и управление профилями питомцев**
- **Просмотр профилей других пользователей**
- **API для взаимодействия с данными (REST API)**

## Запуск с помощью docker compose

### 1. Убедитесь, что на вашем компьютере установлен Docker

Для установки воспользуйтесь официальной документацией для [Linux](https://docs.docker.com/engine/install/), [Mac](https://docs.docker.com/desktop/setup/install/mac-install/) или [Windows](https://docs.docker.com/desktop/setup/install/windows-install/)

### 2. Запуск приложения в docker
Необходимо скачать файл [docker-compose.production.yml](https://github.com/mantulyak-practicum/kittygram_final/blob/main/docker-compose.production.yml) и создать файл с переменными `.env` на основе [образца](https://github.com/mantulyak-practicum/kittygram_final/blob/main/.env.example)\
После этого надо запустить приложение, выполнить миграции и скопировать статику в нужное расположение:

```bash
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
Kittygram будет доступен по адресу http://127.0.0.1:9000/