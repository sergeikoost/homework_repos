1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
⌄
⌄
⌄
⌄
⌄
⌄
⌄
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

## Введение

Для выполнения заданий обязательно ознакомьтесь с инструкцией по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1: Установка Docker и создание образа

### Сценарий выполнения задачи:

1. **Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.**

   ```bash
   sudo apt-get update
   sudo apt-get install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
Установка Docker Compose:

bash
Copy
1
2
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -Po '"tag_name": "\K.*\d')" /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
Если Docker Hub недоступен, создайте файл /etc/docker/daemon.json с содержимым:
json
Copy
1
2
3
⌄
{
  "registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]
}
Перезапустите службу Docker:
bash
Copy
1
sudo systemctl restart docker
Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-apache" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП).
Скачайте образ apache:2.4.
bash
Copy
1
docker pull httpd:2.4
Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы (/usr/local/apache2/htdocs/index.html), на файл index.html с содержимым:
html
Copy
1
2
3
4
5
6
7
8
⌄
⌄
⌄
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
Пример Dockerfile:
dockerfile
Copy
1
2
FROM httpd:2.4
COPY index.html /usr/local/apache2/htdocs/index.html
Соберите и отправьте созданный образ в свой Docker Hub репозиторий с тегом 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП).
Сборка образа:
bash
Copy
1
docker build -t <username>/custom-apache:1.0.0 .
Логин в Docker Hub:
bash
Copy
1
docker login
Отправка образа:
bash
Copy
1
docker push <username>/custom-apache:1.0.0
Предоставьте ответ в виде ссылки на https://hub.docker.com/ <username_repo>/custom-apache/general.
Задача 2: Запуск и управление контейнером
Сценарий выполнения задачи:
Запустите ваш образ custom-apache:1.0.0 командой docker run в соответствии с требованиями:
Имя контейнера: "ФИО-custom-apache-t2"
Контейнер работает в фоне
Контейнер опубликован на порту хост системы 127.0.0.1:8081
bash
Copy
1
docker run --name ФИО-custom-apache-t2 -d -p 127.0.0.1:8081:80 <username>/custom-apache:1.0.0
Не удаляя, переименуйте контейнер в "custom-apache-t2".
bash
Copy
1
docker rename ФИО-custom-apache-t2 custom-apache-t2
Выполните команду:
bash
Copy
1
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8081 ; docker logs custom-apache-t2 -n1 ; docker exec -it custom-apache-t2 base64 /usr/local/apache2/htdocs/index.html
Убедитесь с помощью curl или веб-браузера, что индекс-страница доступна.
bash
Copy
1
curl http://127.0.0.1:8081
Задача 3: Подключение к контейнеру и управление конфигурацией
Сценарий выполнения задачи:
Подключитесь к стандартному потоку ввода/вывода/ошибок контейнера "custom-apache-t2".
bash
Copy
1
docker attach custom-apache-t2
Нажмите комбинацию клавиш Ctrl-C для отсоединения.
Выполните команду docker ps -a и объясните своими словами почему контейнер остановился.
Проверьте список всех контейнеров:
bash
Copy
1
docker ps -a
Объяснение: Контейнер остановился, потому что была нажата комбинация клавиш Ctrl-C, которая отправила сигнал SIGINT и завершила процесс внутри контейнера.
Перезапустите контейнер.
bash
Copy
1
docker restart custom-apache-t2
Зайдите в интерактивный терминал контейнера "custom-apache-t2" с оболочкой bash.
bash
Copy
1
docker exec -it custom-apache-t2 bash
Установите любимый текстовый редактор (vim, nano и т.д.) с помощью apt-get.
bash
Copy
1
apt-get update && apt-get install vim -y
Отредактируйте файл конфигурации Apache (httpd.conf), заменив порт "Listen 80" на "Listen 8081".
Редактирование файла:
bash
Copy
1
vim /usr/local/apache2/conf/httpd.conf
Найдите строку Listen 80 и замените её на Listen 8081.
Запомните(!) и выполните команду apachectl graceful, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:8081 .
Перезагрузка Apache:
bash
Copy
1
apachectl graceful
Проверка доступности портов:
bash
Copy
1
2
curl http://127.0.0.1:80
curl http://127.0.0.1:8081
Выйдите из контейнера, набрав в консоли exit или Ctrl-D.
Выход из контейнера:
bash
Copy
1
exit
Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8081 , docker port custom-apache-t2, curl http://127.0.0.1:8081 . Кратко объясните суть возникшей проблемы.
Проверка открытых портов:
bash
Copy
1
ss -tlpn | grep 127.0.0.1:8081
Проверка маппинга портов:
bash
Copy
1
docker port custom-apache-t2
Проверка доступности индекс-страницы:
bash
Copy
1
curl http://127.0.0.1:8081
Объяснение: Если запрос к порту 80 не работает, это может быть связано с изменением конфигурации Apache на порт 8081.
Задача 4: Работа с несколькими контейнерами
Сценарий выполнения задачи:
Запустите первый контейнер из образа ubuntu c любым тегом в фоновом режиме, подключив папку текущего рабочего каталога $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
bash
Copy
1
docker run -d -v $(pwd):/data ubuntu
Запустите второй контейнер из образа alpine в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
bash
Copy
1
docker run -d -v $(pwd):/data alpine
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
bash
Copy
1
2
docker exec -it <container_id> bash
echo "Hello from Ubuntu container" > /data/ubuntu_file.txt
Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
bash
Copy
1
echo "Hello from host machine" > host_file.txt
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
bash
Copy
1
2
3
4
docker exec -it <container_id> sh
ls /data
cat /data/ubuntu_file.txt
cat /data/host_file.txt
Задача 5: Использование Docker Compose
Сценарий выполнения задачи:
Создайте отдельную директорию (например /tmp/netology/docker/task5) и два файла внутри неё.
Файл compose.yaml с содержимым:
yaml
Copy
1
2
3
4
5
6
7
⌄
⌄
⌄
version: "3"
services:
  grafana:
    network_mode: host
    image: grafana/grafana:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
Файл docker-compose.yaml с содержимым:
yaml
Copy
1
2
3
4
5
6
⌄
⌄
⌄
version: "3"
services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"
Выполните команду docker compose up -d. Какой из файлов был запущен и почему?
Запуск композиций:
bash
Copy
1
docker compose up -d
Объяснение: По умолчанию используется файл docker-compose.yaml, если он существует. Таким образом, будет запущен сервис redis.
Отредактируйте файл compose.yaml так, чтобы были запущены оба файла.
Модификация файла compose.yaml с использованием директивы include:
yaml
Copy
1
2
3
4
5
6
7
8
9
⌄
⌄
⌄
⌄
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
bash
Copy
1
2
docker tag <image_name> localhost:5000/<image_name>
docker push localhost:5000/<image_name>
Откройте страницу "https://127.0.0.1:3000 " и произведите начальную настройку Grafana (логин и пароль администратора).
Настройка Grafana:
bash
Copy
1
2
# Откройте браузер и перейдите по адресу https://127.0.0.1:3000
# Введите логин и пароль администратора
Откройте страницу "http://127.0.0.1:3000 ", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:
Деплой стека:
yaml
Copy
1
2
3
4
5
6
⌄
⌄
⌄
version: '3'
services:
  apache:
    image: 127.0.0.1:5000/custom-apache
    ports:
      - "9091:80"
Перейдите на страницу "http://127.0.0.1:3000 ", выберите контейнер с Apache и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
Инспектирование контейнера:
bash
Copy
1
2
# В интерфейсе Grafana найдите контейнер с Apache и нажмите "inspect"
# Сделайте скриншот нужного раздела
Удалите любой из манифестов компоуза (например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
Удаление манифеста и перезапуск композиций:
bash
Copy
1
2
3
4
rm compose.yaml
docker compose up -d
# Прочитайте warning и выполните предложенное действие
docker compose down
Эти задачи помогут вам закрепить знания и команды, связанные с установкой Docker, созданием и управлением контейнерами, а также использованием Docker Compose для управления многоконтейнерными приложениями.

Copy
1
2

Этот `.md` файл содержит все задачи с подробным описанием и командами, которые необходимо
