# Kittygram

## Описание

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Пользователи могут вводить имя, год рождения, цвет, достижения, а также добавить фото питомца

## Технологии

[![Python](https://img.shields.io/badge/python-3.9-blue?logo=python)](https://www.python.org/)
![Django](https://img.shields.io/badge/DJANGO-3.2.3-darkgreen?logo=django&logoColor=white)
![Gunicorn](https://img.shields.io/badge/GUNICORN-blue?logo=gunicorn&logoColor=green)
![djangorestframework](https://img.shields.io/badge/DJANGORESTFRAMEWORK-3.12.14-blue?logo=django&logoColor=white)
![Nginx](https://img.shields.io/badge/NGINX-269539.svg?&style=flat&logo=nginx&logoColor=white)

## Установка и запуск

- Клонируйте репозитоий:

```
git clone https://
```

- Перейдите в папку проекта:

```
cd infra_sprint1
```

### Запуск backend части на сервере

- Создайте и активируйте виртуальное окружение:

```
python -m venv venv
source venv/bin/activate
```

```
python -m pip install --upgrade pip
```

- Установите зависимости:

```
pip install -r requirements.txt
```

- Сделайте миграции:

```
cd backend/cats
python manage.py migrate
```

- Создайте суперпользователя:

```
python manage.py createsuperuser
```

- Отредактируйте файл settings.py:

```
# Вместо xxx.xxx.xxx.xxx укажите IP вашего сервера.
ALLOWED_HOSTS = ['xxx.xxx.xxx.xxx', '127.0.0.1', 'localhost'] 
```

#### Запуск frontend части на сервере

- Установте на сервер Node.js:

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs 
```

- Установите зависимости для frontend:
```
cd infra_sprint1/frontend/
npm i
```

#### Установка и запуск Gunicorn

- Установите пакет gunicorn:

```
pip install gunicorn==20.1.0
```

- Создайте файл конфигурации для автозапуска WSGi-сервера:

```
sudo nano /etc/systemd/system/gunicorn_kittygram.service
```

- Внесите в него следующие настройки:

```
[Unit]
Description=gunicorn daemon
After=network.target
[Service]
User= <имя пользователя>

WorkingDirectory=/home/<имя пользователя>/infra_sprint1/backend/

ExecStart=/home/<имя пользователя>/infra_sprint1/backend/venv/bin/gunicorn --bind 0.0.0.0:8080 kittygram_backend.wsgi

[Install]

WantedBy=multi-user.target
```

- Запустите gunicorn:

```
sudo systemctl start gunicorn_kittygram
sudo systemctl enable gunicorn_kittygram
```

### Сборка статики для админки

```
python manage.py collectstatic
```

- Скопируйте директорию static_backend/ в директорию /var/www/kittygram/

```
sudo cp -r /home/yc-user/infra_sprint1/backend/static_backend/ /var/www/kittigram/
sudo systemctl restart gunicorn_kittygram
```

#### Установка и настройка Nginx

- Запустите сборку frontend приложения:

```
cd infra_sprrint1/frontend
npm run build
```

- Перейдите в файл конфигурации веб-сервера и измените его настройки на следующие:

```
server {

    server_name server_name <публичный-IP-адрес> <доменное-имя>;

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:8080;
    }

    location /media/ {
        alias /var/www/infra_sprint1/media/;
    }

    location / {
    root    /var/www/infra_sprint1;
    index   index.html index.htm;
    try_files  $uri /index.html;
    }

}
```

- Перезарузите Nginx:

```
sudo nginx -t
sudo systemctl reload nginx
```

- Откройте порты для фаервола и активируйте его:

```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```

- (Опционально) Получите SSL-сертификат для вашего доменного имени с помощью Certbot:

```
sudo apt install snapd
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot 
sudo certbot --nginx
```

#### Автор

[Светлана Шатунова](https://github.com/SvShatunova)
