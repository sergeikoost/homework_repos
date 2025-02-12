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

![homework2_task1 2 1](https://github.com/user-attachments/assets/1b1628c1-9b2e-453a-bdec-c6175f438d49)


Создал mysql базу на хостовой машине т.к. базы не рекомендуется запускать в докере, поэтому в DB_HOST=10.129.0.6, добавил туда юзера и прочее:

CREATE USER 'app'@'localhost' IDENTIFIED BY 'very_strong';
GRANT ALL PRIVILEGES ON example.* TO 'app'@'localhost';
FLUSH PRIVILEGES;


В файле .dockerignore :
Исключаем виртуальное окружение
Исключаем кэш Python
Исключаем скомпилированные файлы Python
Исключаем логи и другие временные файлы

![task1 1_dockerignore](https://github.com/user-attachments/assets/d1c39af2-5aca-4496-87ca-5e04f79cde2d)


Образ собрался без проблем, только немного ругался что [notice] A new release of pip is available: 23.0.1 -> 25.0

![homework2_task1 2 2](https://github.com/user-attachments/assets/4b915cac-3fba-4e86-a75f-a3d607339c3e)

Запустил контейнер, приложение работает но с одним но, я не смог попасть извне на внешний адрес хостовой машины, хотя HTTP запросы вижу:

![homework2_task1 2 3](https://github.com/user-attachments/assets/2392489a-8cdb-46b2-9ca2-4961fa5f5ab3)


## Задача 2

![homework2_task2 2](https://github.com/user-attachments/assets/72a4a874-62ec-45d1-b6df-ac3fad0fce45)
