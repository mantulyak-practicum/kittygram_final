# Как запустить проект:

## Локальный запуск
### Клонировать репозиторий и перейти в него в командной строке:

```bash
git clone https://github.com/mantulyak-practicum/kittygram_final.git
cd kittygram_final
```

### Cоздать и активировать виртуальное окружение:

```bash
python3 -m venv venv
source venv/bin/activate   # Для macOS/Linux
venv\Scripts\activate      # Для Windows
pip install --upgrade pip
pip install -r requirements.txt
```

### Создать файл `.env` и определить в нем необходимые переменные, запустить проект и выполнить миграции
Если в `.env` не определить переменную `DB_ENGINE`, то бекэнд запустится с движком СУБД sqlite3

```bash
cp .env.example .env
cd backend
python3 manage.py migrate
python3 manage.py runserver
python3 manage.py collectstatic
```

## Запуск с помощью docker
### Убедитесь, что на вашем компьютере установлен Docker

Для установки воспользуйтесь официальной документацией для [Linux](https://docs.docker.com/engine/install/), [Mac](https://docs.docker.com/desktop/setup/install/mac-install/) или [Windows](https://docs.docker.com/desktop/setup/install/windows-install/)

### Запуск в docker c PostgreSQL
Чтобы запустить бекэнд в docker необходимо создать файл с переменными `.env.` по [образцу](https://github.com/mantulyak-practicum/kittygram_final/blob/main/.env.example)\

Создаем сеть и volume
```bash
sudo docker network create kittygram-backend
sudo docker volume create pg_data
```
Запускаем контейнеры, делаем миграции и собираем статику бекэнда
```bash
sudo docker run --name db \
    -d --env-file .env \
    -e POSTGRES_DB=${DB_NAME} \
    -e POSTGRES_USER=${DB_USER} \
    -e POSTGRES_PASSWORD=${DB_PASSWORD} \
    -v pg_data:/var/lib/postgresql/data \
    --network kittygram-backend \
    postgres:13.10
sudo docker run --name backend \
    -d --env-file .env \
    --network kittygram-backend \
    -p 8000:8000 \
    mantuliak/kittygram_backend
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
```
Бекэнд Kittygram будет доступен по адресу http://127.0.0.1:8000/


### Запуск в docker c SQLite3
Если в `.env.` не указвать переменные `DB_*`, то бекэнд запустится с движком БД sqlite3 и контейнер с postgres запускать не обязательно
```bash
sudo docker run --name backend \
    -d --env-file .env \
    --network kittygram-backend \
    -p 8000:8000 \
    mantuliak/kittygram_backend
```
