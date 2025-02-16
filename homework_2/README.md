## Задача 0


1. Убедитесь что у вас НЕ(!) установлен ```docker-compose```, для этого получите следующую ошибку от команды ```docker-compose --version```

docker-compose был установлен, удаляем:


![homework2_task0](https://github.com/user-attachments/assets/c73faa0e-1324-4f38-a402-908f9055df44)


2. Убедитесь что у вас УСТАНОВЛЕН ```docker compose```(без тире) версии не менее v2.24.X, для это выполните команду ```docker compose version```  

Установка Docker Compose версии без тире (т.е. docker compose как встроенная команда, а не отдельный бинарник docker-compose) происходит автоматически при установке Docker Engine начиная с версии 20.10. В этой версии команда compose была интегрирована в основное ядро Docker. У меня docker26.1.3, устанавливать docker compose(без тире) нет необходимости. 

![homework2_task0 2](https://github.com/user-attachments/assets/bd1fb781-4ccb-473c-8bbc-f2d9676814dd)

Но все же если что-то не так и не работает то можно установить плагин так:

mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose

После чего docker compose будет работать корректно.


## Задача 1

1. Сделайте в своем github пространстве fork [репозитория](https://github.com/netology-code/shvirtd-example-python/blob/main/README.md).
   Примечание: В связи с доработкой кода python приложения. Если вы уверены что задание выполнено вами верно, а код python приложения работает с ошибкой то используйте вместо main.py файл not_tested_main.py(просто измените CMD)

Создание fork репозитория
Перешел на страницу shvirtd-example-python .
Нажал кнопку "Fork" в правом верхнем углу страницы.
Это создаст копию репозитория в вашем пространстве GitHub.

2. Создайте файл с именем Dockerfile.python для сборки данного проекта(для 3 задания изучите https://docs.docker.com/compose/compose-file/build/ ). Используйте базовый образ python:3.9-slim. Обязательно используйте конструкцию COPY . . в Dockerfile. Не забудьте исключить ненужные в имадже файлы с помощью dockerignore. Протестируйте корректность сборки.

![task1 1_dockerfile](https://github.com/user-attachments/assets/780e7a80-224a-4f4a-a70d-bdec6baea863)


Создал mysql базу на хостовой машине т.к. базы не рекомендуется запускать в докере, поэтому в DB_HOST=10.129.0.23, добавил туда юзера, базу. Также пришлось изменить mysql.conf файл и внести туда bind-address = 0.0.0.0 чтобы sql слушал все интерфейсы:

CREATE USER 'app'@'localhost' IDENTIFIED BY 'very_strong';
GRANT ALL PRIVILEGES ON example.* TO 'app'@'localhost';
FLUSH PRIVILEGES;


В файле .dockerignore :
Исключаем виртуальное окружение
Исключаем кэш Python
Исключаем скомпилированные файлы Python
Исключаем логи и другие временные файлы

![task1 1_dockerignore](https://github.com/user-attachments/assets/d1c39af2-5aca-4496-87ca-5e04f79cde2d)


Собираем образ, образ собирается успешно:


![task1 1_imagecreate](https://github.com/user-attachments/assets/9f642827-542f-4bb2-bcba-f44f9178874a)



Запустил контейнер, приложение работает:

![task1 1_flusk2](https://github.com/user-attachments/assets/5ad1df61-3c30-4fda-8e06-e331ec446c86)

![task1 1_flusk](https://github.com/user-attachments/assets/854dfcea-7bd0-4cc2-bfe3-596910c37662)





## Задача 2

Все сделал согласно инструкции на yandex cloud
![homework2_task2 2](https://github.com/user-attachments/assets/72a4a874-62ec-45d1-b6df-ac3fad0fce45)

## Задача 3


Изучите файл "proxy.yaml"
Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
Опишите в файле compose.yaml следующие сервисы:


 - Подключение proxy.yaml:

version: '3.8'
```

include:
./proxy.yaml
```


 - Сервис web:
```

services:
  web:
    build:
      context: .
      dockerfile: Dockerfile.python
    networks:
      backend:
        ipv4_address: 172.20.0.5
    restart: on-failure
    environment:
      DB_HOST: 172.20.0.10
      DB_USER: root
      DB_PASSWORD: very_strong
      DB_NAME: example
      DB_PORT: 3306
    ports:
      - "5000:5000"
    depends_on:
      - db

```

 - Сервис db:
```

  db:
    image: mysql:8
    networks:
      backend:
        ipv4_address: 172.20.0.10
    restart: on-failure
    environment:
      MYSQL_ROOT_PASSWORD: very_strong
      MYSQL_USER: app
      MYSQL_PASSWORD: very_strong
      MYSQL_DATABASE: example
    ports:
      - "3307:3306"
    volumes:
      - ./var/lib/mysql:/var/lib/mysql

```

 - Сеть:
```

networks:
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/24
```


#Общая логика в моем compose.yaml
Запускается контейнер db:
```

Инициализируется MySQL. Данные сохраняются в директорию ./var/lib/mysql. MySQL слушает порт 3306 внутри контейнера и доступен на хосте через порт 3307.
 Также я использовал mysql на хостовой машине, базы соответсвенно там же.
```

Запускается контейнер web:
```

Собирается образ из Dockerfile.python. Приложение пытается подключиться к базе данных на хосте 172.20.0.10 который является сервисом db.
 Если подключение успешно, приложение начинает работать на порту 5000.
```

![task3 1_curl](https://github.com/user-attachments/assets/5bfc9391-930f-437e-af4a-d0ce6cd96298)


Подключитесь к БД mysql с помощью команды docker exec -ti <имя_контейнера> mysql -uroot -p<пароль root-пользователя>(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.


Поскольку базу я сделал локально то и проверял все я локально, если это проблема могу переделать:

![task3 2_sql](https://github.com/user-attachments/assets/b3a2527c-806a-486a-85a7-99f46a0896b7)


## Задача 4

Ссылка на репозиторий github:   https://github.com/sergeikoost/repo_homework2

Написал bash скрипт:

```
REPO_URL="https://github.com/sergeikoost/repo_homework2.git"
TARGET_DIR="/opt/repo_homework2"

git clone "$REPO_URL" "$TARGET_DIR"

cd "$TARGET_DIR" || exit

docker compose up -d

```

![homework2_task4 2](https://github.com/user-attachments/assets/3fde53a8-44b6-4c2f-82c2-38d0e22e6194)


Проект запустился, скриншот с https://check-host.net/check-http:

![homework2_task4](https://github.com/user-attachments/assets/9225dfe3-13ae-488d-8524-e842019808c9)


## Задача 6


Скачайте docker образ hashicorp/terraform:latest:

![homework2_task6](https://github.com/user-attachments/assets/49430449-b044-4682-b9eb-325af1cfae88)

Нашел где находится файл terraform при помощи dive:

![homework2_task6 1](https://github.com/user-attachments/assets/1f039918-1bec-44b8-94da-9417e8977e15)

digest sha256:da25c3c268493bc8d1313c7698a81a97a99c917ae09a248795e969d82cb53f65

Создаем образ:

docker save hashicorp/terraform -o homework2_task6.tar

Распаковываем и ищем необходимый слой, как найдем распаковываем и его:

![homework2_task6 2](https://github.com/user-attachments/assets/f85877fb-f374-4658-b890-3e5b67ae04b0)

