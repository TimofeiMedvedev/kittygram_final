## Спринт 17: Проект kittygram_final

## **Описание проекта**

Проект находится по адресу: https://the-oneproject.zapto.org/

## Локальный запуск проекта

Клонировать репозиторий по SSH и перейти в него в командной строке:

```bash
git clone git@github.com:TimofeiMedvedev/kittygram_final.git
cd kittygram_final
```
Установите [docker compose](https://www.docker.com/) на свой компьютер.

## .env

В корне проекта создайте файл .env и пропишите в него свои данные.

Пример:

```apache
SECRET_KEY = 'my_secret_key'
POSTGRES_DB=dbnew
POSTGRES_USER=dbnewuser
POSTGRES_PASSWORD=dwnewpassword
DB_NAME=dbnew
DB_HOST=db
DB_PORT=5432 
DEBUG=False
ALLOWED_HOSTS = '*******.****.net, **.**.**.**, 127.0.0.1, localhost'
```

Запустите проект через docker-compose:

```bash
docker compose -f docker-compose.yml up --build -d
```

Выполнить миграции:

```bash
docker compose -f docker-compose.yml exec backend python manage.py migrate
```

Соберите статику и скопируйте ее:

```bash
docker compose -f docker-compose.yml exec backend python manage.py collectstatic  && \
docker compose -f docker-compose.yml exec backend cp -r /app/static_backend/. /backend_static/
```

## Workflow

Для использования Continuous Integration (CI) и Continuous Deployment (CD): в репозитории GitHub Actions `Settings/Secrets/Actions` прописать Secrets - переменные окружения для доступа к сервисам:

```
DOCKER_USERNAME                # имя пользователя в DockerHub
DOCKER_PASSWORD                # пароль пользователя в DockerHub
HOST                           # ip_address сервера
USER                           # имя пользователя
SSH_KEY                        # приватный ssh-ключ (cat ~/.ssh/id_rsa)
PASSPHRASE                     # кодовая фраза (пароль) для ssh-ключа

TELEGRAM_TO                    # id телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN                 # токен бота (получить токен можно у @BotFather, /token, имя бота)
```

При push в ветку main автоматически отрабатывают сценарии:

* *tests* - проверка кода на соответствие стандарту PEP8 и запуск pytest. Дальнейшие шаги выполняются только если push был в ветку main;
* *build\_and\_push\_to\_docker\_hub* - сборка и доставка докер-образов на DockerHub
* *deploy* - автоматический деплой проекта на боевой сервер. Выполняется копирование файлов из DockerHub на сервер;
* *send\_message* - отправка уведомления в Telegram.

Автор *Тимофей Медведев**.