Задача 1: Установка Docker и создание образа

**Установка Docker и Docker Compose**

.. code-block:: bash

   sudo apt-get update
   sudo apt-get install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker

**Скачивание образа nginx:1.21.1**

.. code-block:: bash

   docker pull nginx:1.21.1

**Создание Dockerfile для замены дефолтной индекс-страницы**

.. code-block:: dockerfile

   FROM nginx:1.21.1
   COPY index.html /usr/share/nginx/html/index.html

**Сборка и отправка образа в Docker Hub (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП)**

.. code-block:: bash

   docker build -t <username>/custom-nginx:1.0.0 .
   docker login
   docker push <username>/custom-nginx:1.0.0

### Задача 2: Запуск и управление контейнером

**Запуск контейнера custom-nginx:1.0.0**

.. code-block:: bash

   docker run --name ФИО-custom-nginx-t2 -d -p 127.0.0.1:8080:80 <username>/custom-nginx:1.0.0

**Переименование контейнера**

.. code-block:: bash

   docker rename ФИО-custom-nginx-t2 custom-nginx-t2

**Выполнение команды для получения информации о контейнере**

.. code-block:: bash

   date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html

**Проверка доступности индекс-страницы**

.. code-block:: bash

   curl http://127.0.0.1:8080

### Задача 3: Подключение к контейнеру и управление конфигурацией

**Подключение к стандартному потоку ввода/вывода/ошибок контейнера**

.. code-block:: bash

   docker attach custom-nginx-t2
   Ctrl-C

**Перезапуск контейнера**

.. code-block:: bash

   docker restart custom-nginx-t2

**Подключение к терминалу контейнера**

.. code-block:: bash

   docker exec -it custom-nginx-t2 bash

**Установка текстового редактора внутри контейнера**

.. code-block:: bash

   apt-get update && apt-get install vim -y

**Изменение конфигурации Nginx**

.. code-block:: bash

   sed -i 's/listen 80;/listen 81;/' /etc/nginx/conf.d/default.conf
   nginx -s reload

**Проверка изменений**

.. code-block:: bash

   curl http://127.0.0.1:80
   curl http://127.0.0.1:81

**Выйти из контейнера**

.. code-block:: bash

   exit

**Проверка конфигурации контейнера**

.. code-block:: bash

   ss -tlpn | grep 127.0.0.1:8080
   docker port custom-nginx-t2
   curl http://127.0.0.1:8080

**Удаление запущенного контейнера без остановки**

.. code-block:: bash

   docker rm -f custom-nginx-t2

### Задача 4: Работа с несколькими контейнерами

**Запуск контейнера CentOS**

.. code-block:: bash

   docker run -d -v $(pwd):/data centos

**Запуск контейнера Debian**

.. code-block:: bash

   docker run -d -v $(pwd):/data debian

**Создание файла внутри контейнера CentOS**

.. code-block:: bash

   docker exec -it <container_id> touch /data/newfile.txt

**Добавление файла на хостовой машине**

.. code-block:: bash

   touch newfile_host.txt

**Просмотр файлов внутри контейнера Debian**

.. code-block:: bash

   docker exec -it <container_id> ls /data
   docker exec -it <container_id> cat /data/newfile.txt

### Задача 5: Использование Docker Compose

**Создание директории и файлов compose.yaml и docker-compose.yaml**

.. code-block:: yaml

   # compose.yaml
   version: "3"
   services:
     portainer:
       network_mode: host
       image: portainer/portainer-ce:latest
       volumes:
         - /var/run/docker.sock:/var/run/docker.sock

   # docker-compose.yaml
   version: "3"
   services:
     registry:
       image: registry:2
       ports:
         - "5000:5000"

**Запуск композиций**

.. code-block:: bash

   docker compose up -d

**Отправка образа в локальный реестр**

.. code-block:: bash

   docker tag <image_name> localhost:5000/<image_name>
   docker push localhost:5000/<image_name>

**Настройка Portainer и деплой стека**

.. code-block:: bash

   curl http://127.0.0.1:9000/#!/home
   # Выберите ваше local окружение
   # Перейдите на вкладку "stacks" и задеплойте следующий компоуз:
   version: '3'
   services:
     nginx:
       image: 127.0.0.1:5000/custom-nginx
       ports:
         - "9090:80"

**Проверка конфигурации контейнера**

.. code-block:: bash

   docker inspect <container_id>

**Удаление манифестов компоуза и обработка предупреждений**

.. code-block:: bash

   docker compose down
