# Kittygram

## Описание

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Пользователи могут вводить имя, год рождения, цвет, достижения, а также добавить фото питомца

## Технологии

- Django==3.2.3
- djangorestframework==3.12.4
- djoser==2.1.0
- webcolors==1.11.1
- psycopg2-binary==2.9.3
- Pillow==9.0.0
- pytest==6.2.4
- pytest-django==4.4.0
- pytest-pythonpath==0.7.3
- PyYAML==6.0

### Установка и запуск проекта

1. Клонирование кода приложения с GitHub.
    - git clone SSH-ссылка

2. Создать файл .env.example в котором перечислены все переменные окружения.
    - POSTGRES_DB=
    - POSTGRES_USER=
    - POSTGRES_PASSWORD=

3. Проверить созданные образы.
    - docker image ls

4. Запустить Docker Compose на компьютере.
    - docker compose -f docker-compose.production.yml up

5. Собрать статику.
    - docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    - docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/

6. Применить миграции.
    - docker compose -f docker-compose.production.yml exec backend python manage.py migrate

#### Автор

Светлана Шатунова
