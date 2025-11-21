RESTful API для книжного магазина
Это RESTful API, разработанное на Django Rest Framework. Поддерживает CRUD-операции для моделей Author, Genre и Book. Реализована аутентификация через JWT-токены, пагинация для списков и документация через Swagger (drf-spectacular). База данных — SQLite.
Проект готов к интеграции с фронтендом (React/Vue.js) или мобильными приложениями.

Инструкция, как развернуть проект на другом ПК.

Шаг 1: Клонируйте репозиторий
Откройте терминал и выполните:
clone https://github.com/ZEDDTAY8/RESTfulAPI.git
cd RESTfulAPI

Шаг 2: Создайте виртуальное окружение
На Linux:
python3 -m venv venv
source venv/bin/activate
(Если venv не установлен: sudo apt install python3-venv)

Шаг 3: Установите зависимости
pip install -r requirements.txt

Шаг 4: Создайте миграции и примените их
python3 manage.py makemigrations
python3 manage.py migrate

Шаг 5: Создайте суперюзера (для получения JWT-токена)
python3 manage.py createsuperuser
Введите логин, email (опционально) и пароль.

Шаг 6: Запустите сервер
python3 manage.py runserver

Проверка работоспособности

Шаг 1: Перейдите в Swagger
Зайдите на: http://127.0.0.1:8000/swagger/
Увидите документацию API с зелёными кнопками для эндпоинтов (/api/books/, /api/authors/, /api/genres/).

Шаг 2: Получите JWT-токен
В Swagger найдите POST /api/token/:

Нажмите Try it out.
Введите ваш username и password от суперюзера.
Нажмите Execute.
Скопируйте "access" токен из ответа.

Шаг 3: Авторизуйтесь в Swagger
Наверху справа нажмите кнопку Authorize:

В поле вставьте <ваш_access_токен>.
Нажмите Authorize → Close.

Теперь вы авторизованы для всех запросов.

Шаг 4: Протестируйте CRUD

Чтение (GET): GET /api/books/ → Execute. Увидите пустой список с пагинацией (count: 0, next: null).
Создание (POST): POST /api/genres/ → Try it out → Вставьте {"name": "Фантастика"} → Execute (201 Created).
Аналогично добавьте автора: POST /api/authors/ → {"name": "Автор", "bio": "Био"}.
Добавьте книгу: POST /api/books/ → {"title": "Книга", "author": 1, "genre": 1, "price": "100.00", "published_date": "2025-01-01"}.
Обновление (PUT): GET /api/books/1/ → скопируйте данные, измените и отправьте PUT /api/books/1/.
Удаление (DELETE): DELETE /api/books/1/ → Execute (204 No Content).

Шаг 5: Проверьте пагинацию
Добавьте 12 книг (через POST, меняя title).
Затем GET /api/books/ → увидите 10 книг, count:12, next: ...?page=2.
В параметрах впишите page=2 → Execute → увидите оставшиеся 2.