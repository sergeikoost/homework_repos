Конечно, давайте создадим аналогичные задачи с другими переменными и условиями, но используя те же знания и команды, что были показаны в задачах 1 и 2.

Задача 3: Подключение к контейнеру и управление конфигурацией
Сценарий выполнения задачи:

Запустите образ custom-apache на порту 8081.
Создайте Dockerfile для создания образа custom-apache, который будет использовать Apache HTTP Server вместо Nginx.
.. code-block:: dockerfile
FROM httpd:2.4
COPY index.html /usr/local/apache2/htdocs/
Содержимое файла index.html:
.. code-block:: html
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
Соберите и отправьте образ в Docker Hub (если доступно):
.. code-block:: bash
docker build -t <username>/custom-apache:1.0.0 .
docker login
docker push <username>/custom-apache:1.0.0
Запустите контейнер:
.. code-block:: bash
docker run --name custom-apache-t3 -d -p 127.0.0.1:8081:80 <username>/custom-apache:1.0.0
Подключитесь к стандартному потоку ввода/вывода/ошибок контейнера "custom-apache-t3".
Используйте команду docker attach для подключения:
.. code-block:: bash
docker attach custom-apache-t3
Нажмите комбинацию клавиш Ctrl-C для отсоединения.
Перезапустите контейнер.
Перезапуск контейнера:
.. code-block:: bash
docker restart custom-apache-t3
Зайдите в интерактивный терминал контейнера "custom-apache-t3" с оболочкой bash.
Подключение к терминалу контейнера:
.. code-block:: bash
docker exec -it custom-apache-t3 bash
Установите любимый текстовый редактор (vim, nano и т.д.) с помощью apt-get.
Установка редактора vim:
.. code-block:: bash
apt-get update && apt-get install vim -y
Отредактируйте файл конфигурации Apache (httpd.conf), заменив порт "Listen 80" на "Listen 8081".
Редактирование файла:
.. code-block:: bash
vim /usr/local/apache2/conf/httpd.conf
Найдите строку Listen 80 и замените её на Listen 8081.
Перезагрузите Apache и проверьте доступность портов.
Перезагрузка Apache:
.. code-block:: bash
apachectl graceful
Проверка доступности портов:
.. code-block:: bash
curl http://127.0.0.1:8081
Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
Выход из контейнера:
.. code-block:: bash
exit
Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8081 , docker port custom-apache-t3, curl http://127.0.0.1:8081 . Кратко объясните суть возникшей проблемы.
Проверка открытых портов:
.. code-block:: bash
ss -tlpn | grep 127.0.0.1:8081
Проверка маппинга портов:
.. code-block:: bash
docker port custom-apache-t3
Проверка доступности индекс-страницы:
.. code-block:: bash
curl http://127.0.0.1:8081
Объяснение: Если запрос к порту 80 не работает, это может быть связано с изменением конфигурации Apache на порт 8081.
Задача 4: Работа с несколькими контейнерами
Сценарий выполнения задачи:

Запустите первый контейнер из образа ubuntu c любым тегом в фоновом режиме, подключив папку текущего рабочего каталога $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
Запуск контейнера:
.. code-block:: bash
docker run -d -v $(pwd):/data ubuntu
Запустите второй контейнер из образа alpine в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
Запуск контейнера:
.. code-block:: bash
docker run -d -v $(pwd):/data alpine
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
Подключение к контейнеру и создание файла:
.. code-block:: bash
docker exec -it <container_id> bash
echo "Hello from Ubuntu container" > /data/ubuntu_file.txt
Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
Создание файла на хостовой машине:
.. code-block:: bash
echo "Hello from host machine" > host_file.txt
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
Подключение к контейнеру и просмотр файлов:
.. code-block:: bash
docker exec -it <container_id> sh
ls /data
cat /data/ubuntu_file.txt
cat /data/host_file.txt
Задача 5: Использование Docker Compose
Сценарий выполнения задачи:

Создайте отдельную директорию (например /tmp/netology/docker/task5) и два файла внутри неё.
Файл compose.yaml с содержимым:
.. code-block:: yaml
version: "3"
services:
grafana:
network_mode: host
image: grafana/grafana:latest
volumes:
- /var/run/docker.sock:/var/run/docker.sock
Файл docker-compose.yaml с содержимым:
.. code-block:: yaml
version: "3"
services:
redis:
image: redis:latest
ports:
- "6379:6379"
Выполните команду docker compose up -d. Какой из файлов был запущен и почему?
Запуск композиций:
.. code-block:: bash
docker compose up -d
Объяснение: По умолчанию используется файл docker-compose.yaml, если он существует. Таким образом, будет запущен сервис redis.
Отредактируйте файл compose.yaml так, чтобы были запущены оба файла.
Модификация файла compose.yaml с использованием директивы include:
.. code-block:: yaml
version: "3"
services:
include:
- path: ./docker-compose.yaml
grafana:
network_mode: host
image: grafana/grafana:latest
volumes:
- /var/run/docker.sock:/var/run/docker.sock
Выполните в консоли вашей хостовой ОС необходимые команды, чтобы залить образ custom-apache как custom-apache:latest в запущенное вами, локальное registry.
Отправка образа в локальный реестр:
.. code-block:: bash
docker tag <image_name> localhost:5000/<image_name>
docker push localhost:5000/<image_name>
Откройте страницу "https://127.0.0.1:3000 " и произведите начальную настройку Grafana (логин и пароль администратора).
Настройка Grafana:
.. code-block:: bash
Откройте браузер и перейдите по адресу https://127.0.0.1:3000
Введите логин и пароль администратора
Откройте страницу "http://127.0.0.1:3000 ", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:
Деплой стека:
.. code-block:: yaml
version: '3'
services:
apache:
image: 127.0.0.1:5000/custom-apache
ports:
- "9091:80"
Перейдите на страницу "http://127.0.0.1:3000 ", выберите контейнер с Apache и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
Инспектирование контейнера:
.. code-block:: bash
В интерфейсе Grafana найдите контейнер с Apache и нажмите "inspect"
Сделайте скриншот нужного раздела
Удалите любой из манифестов компоуза (например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
Удаление манифеста и перезапуск композиций:
.. code-block:: bash
rm compose.yaml
docker compose up -d
Прочитайте warning и выполните предложенное действие
docker compose down
Эти задачи используют те же основные команды и знания, что и задачи 1 и 2, но с различными переменными и условиями, чтобы продемонстрировать их применение в других сценариях.

Прочитай каждую задачу и сделай аналогичные задачи для тренировки знаний и команд, на основе которых эти задачи решаются
"Задача 1
Сценарий выполнения задачи:
Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: {"registry-mirrors": ["https://mirror.gcr.io ", "https://daocloud.io ", "https://c.163.com/ ", "https://registry.docker-cn.com&quot;]}
Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
скачайте образ nginx:1.21.1;
Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:

<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
Задача 2
Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
имя контейнера "ФИО-custom-nginx-t2"
контейнер работает в фоне
контейнер опубликован на порту хост системы 127.0.0.1:8080
Не удаляя, переименуйте контейнер в "custom-nginx-t2"
Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
Задача 3
Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
Выполните docker ps -a и объясните своими словами почему контейнер остановился.
Перезапустите контейнер
Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. пример источника
Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
Задача 4
Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку текущий рабочий каталог $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
Задача 5
Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. "compose.yaml" с содержимым:
version: "3"
services:
portainer:
network_mode: host
image: portainer/portainer-ce:latest
volumes:
- /var/run/docker.sock:/var/run/docker.sock
"docker-compose.yaml" с содержимым:
version: "3"
services:
registry:
image: registry:2
ports:
- "5000:5000"
И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )
Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)
Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:
version: '3'
services:
nginx:
image: 127.0.0.1:5000/custom-nginx
ports:
- "9090:80"
Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой."
profile
Qwen2.5-Plus
3:06 pm
Конечно! Давайте создадим аналогичные задачи для тренировки знаний и команд, которые используются в исходных задачах. Каждая новая задача будет иметь схожую структуру, но с измененными переменными и условиями.

Задача 1: Установка Docker и создание образа
Сценарий выполнения задачи:

Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
Обновите пакетный менеджер:
.. code-block:: bash
sudo apt-get update
Установите Docker:
.. code-block:: bash
sudo apt-get install docker.io
Запустите и активируйте службу Docker:
.. code-block:: bash
sudo systemctl start docker
sudo systemctl enable docker
Установите Docker Compose (например, через скачивание бинарного файла):
.. code-block:: bash
sudo curl -L "https://github.com/docker/compose/releases/download/$ (curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -Po '"tag_name": "\K.*\d')" /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
Если Docker Hub недоступен, создайте файл /etc/docker/daemon.json с содержимым:
.. code-block:: json
{
"registry-mirrors": ["https://mirror.gcr.io ", "https://daocloud.io ", "https://c.163.com/ ", "https://registry.docker-cn.com&quot;]
}
Перезапустите службу Docker:
.. code-block:: bash
sudo systemctl restart docker
Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-apache" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП).
Скачайте образ apache:2.4.
.. code-block:: bash
docker pull httpd:2.4
Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы (/usr/local/apache2/htdocs/index.html), на файл index.html с содержимым:
.. code-block:: html
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>

Пример Dockerfile:
.. code-block:: dockerfile
FROM httpd:2.4
COPY index.html /usr/local/apache2/htdocs/index.html
Соберите и отправьте созданный образ в свой Docker Hub репозиторий с тегом 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
Сборка образа:
.. code-block:: bash
docker build -t <username>/custom-apache:1.0.0 .
Логин в Docker Hub:
.. code-block:: bash
docker login
Отправка образа:
.. code-block:: bash
docker push <username>/custom-apache:1.0.0
Предоставьте ответ в виде ссылки на https://hub.docker.com/ <username_repo>/custom-apache/general.
Задача 2: Запуск и управление контейнером
Сценарий выполнения задачи:

Запустите ваш образ custom-apache:1.0.0 командой docker run в соответствии с требованиями:
Имя контейнера: "ФИО-custom-apache-t2"
Контейнер работает в фоне
Контейнер опубликован на порту хост системы 127.0.0.1:8081
.. code-block:: bash
docker run --name ФИО-custom-apache-t2 -d -p 127.0.0.1:8081:80 <username>/custom-apache:1.0.0
Не удаляя, переименуйте контейнер в "custom-apache-t2".
.. code-block:: bash
docker rename ФИО-custom-apache-t2 custom-apache-t2
Выполните команду:
.. code-block:: bash
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8081 ; docker logs custom-apache-t2 -n1 ; docker exec -it custom-apache-t2 base64 /usr/local/apache2/htdocs/index.html
Убедитесь с помощью curl или веб-браузера, что индекс-страница доступна.
.. code-block:: bash
curl http://127.0.0.1:8081
Задача 3: Подключение к контейнеру и управление конфигурацией
Сценарий выполнения задачи:

Подключитесь к стандартному потоку ввода/вывода/ошибок контейнера "custom-apache-t2".
Используйте команду docker attach для подключения:
.. code-block:: bash
docker attach custom-apache-t2
Нажмите комбинацию клавиш Ctrl-C для отсоединения.
Выполните команду docker ps -a и объясните своими словами почему контейнер остановился.
Проверьте список всех контейнеров:
.. code-block:: bash
docker ps -a
Объяснение: Контейнер остановился, потому что была нажата комбинация клавиш Ctrl-C, которая отправила сигнал SIGINT и завершила процесс внутри контейнера.
Перезапустите контейнер.
Перезапуск контейнера:
.. code-block:: bash
docker restart custom-apache-t2
Зайдите в интерактивный терминал контейнера "custom-apache-t2" с оболочкой bash.
Подключение к терминалу контейнера:
.. code-block:: bash
docker exec -it custom-apache-t2 bash
Установите любимый текстовый редактор (vim, nano и т.д.) с помощью apt-get.
Установка редактора vim:
.. code-block:: bash
apt-get update && apt-get install vim -y
Отредактируйте файл конфигурации Apache (httpd.conf), заменив порт "Listen 80" на "Listen 8081".
Редактирование файла:
.. code-block:: bash
vim /usr/local/apache2/conf/httpd.conf
Найдите строку Listen 80 и замените её на Listen 8081.
Запомните(!) и выполните команду apachectl graceful, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:8081 .
Перезагрузка Apache:
.. code-block:: bash
apachectl graceful
Проверка доступности портов:
.. code-block:: bash
curl http://127.0.0.1:80
curl http://127.0.0.1:8081
Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
Выход из контейнера:
.. code-block:: bash
exit
Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8081 , docker port custom-apache-t2, curl http://127.0.0.1:8081 . Кратко объясните суть возникшей проблемы.
Проверка открытых портов:
.. code-block:: bash
ss -tlpn | grep 127.0.0.1:8081
Проверка маппинга портов:
.. code-block:: bash
docker port custom-apache-t2
Проверка доступности индекс-страницы:
.. code-block:: bash
curl http://127.0.0.1:8081
Объяснение: Если запрос к порту 80 не работает, это может быть связано с изменением конфигурации Apache на порт 8081.
Задача 4: Работа с несколькими контейнерами
Сценарий выполнения задачи:

Запустите первый контейнер из образа ubuntu c любым тегом в фоновом режиме, подключив папку текущего рабочего каталога $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
Запуск контейнера:
.. code-block:: bash
docker run -d -v $(pwd):/data ubuntu
Запустите второй контейнер из образа alpine в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
Запуск контейнера:
.. code-block:: bash
docker run -d -v $(pwd):/data alpine
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
Подключение к контейнеру и создание файла:
.. code-block:: bash
docker exec -it <container_id> bash
echo "Hello from Ubuntu container" > /data/ubuntu_file.txt
Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
Создание файла на хостовой машине:
.. code-block:: bash
echo "Hello from host machine" > host_file.txt
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
Подключение к контейнеру и просмотр файлов:
.. code-block:: bash
docker exec -it <container_id> sh
ls /data
cat /data/ubuntu_file.txt
cat /data/host_file.txt
Задача 5: Использование Docker Compose
Сценарий выполнения задачи:

Создайте отдельную директорию (например /tmp/netology/docker/task5) и два файла внутри неё.
Файл compose.yaml с содержимым:
.. code-block:: yaml
version: "3"
services:
grafana:
network_mode: host
image: grafana/grafana:latest
volumes:
- /var/run/docker.sock:/var/run/docker.sock
Файл docker-compose.yaml с содержимым:
.. code-block:: yaml
version: "3"
services:
redis:
image: redis:latest
ports:
- "6379:6379"
Выполните команду docker compose up -d. Какой из файлов был запущен и почему?
Запуск композиций:
.. code-block:: bash
docker compose up -d
Объяснение: По умолчанию используется файл docker-compose.yaml, если он существует. Таким образом, будет запущен сервис redis.
Отредактируйте файл compose.yaml так, чтобы были запущены оба файла.
Модификация файла compose.yaml с использованием директивы include:
.. code-block:: yaml
version: "3"
services:
include:
- path: ./docker-compose.yaml
grafana:
network_mode: host
image: grafana/grafana:latest
volumes:
- /var/run/docker.sock:/var/run/docker.sock
Выполните в консоли вашей хостовой ОС необходимые команды, чтобы залить образ custom-apache как custom-apache:latest в запущенное вами, локальное registry.
Отправка образа в локальный реестр:
.. code-block:: bash
docker tag <image_name> localhost:5000/<image_name>
docker push localhost:5000/<image_name>
Откройте страницу "https://127.0.0.1:3000 " и произведите начальную настройку Grafana (логин и пароль администратора).
Настройка Grafana:
.. code-block:: bash
Откройте браузер и перейдите по адресу https://127.0.0.1:3000
Введите логин и пароль администратора
Откройте страницу "http://127.0.0.1:3000 ", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:
Деплой стека:
.. code-block:: yaml
version: '3'
services:
apache:
image: 127.0.0.1:5000/custom-apache
ports:
- "9091:80"
Перейдите на страницу "http://127.0.0.1:3000 ", выберите контейнер с Apache и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
Инспектирование контейнера:
.. code-block:: bash
В интерфейсе Grafana найдите контейнер с Apache и нажмите "inspect"
Сделайте скриншот нужного раздела
Удалите любой из манифестов компоуза (например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
Удаление манифеста и перезапуск композиций:
.. code-block:: bash
rm compose.yaml
docker compose up -d
Прочитайте warning и выполните предложенное действие
docker compose down
Эти задачи помогут вам закрепить знания и команды, связанные с установкой Docker, созданием и управлением контейнерами, а также использованием Docker Compose для управления многоконтейнерными приложениями.
