Задача №1.
Ссылка на репозиторий Docker hub: https://hub.docker.com/repository/docker/sergeikoost1/custom-nginx/general;

Задача №2.
![Задача 2](https://github.com/user-attachments/assets/1546058e-754b-4235-8f69-c661309aaa18)

Задача №3.

Шаги 1-5:

![Задача 3, пункт 1-5](https://github.com/user-attachments/assets/8f1d561f-e418-4423-a100-195840d63769)

Пункт 3: цель контейнера - выполнить работу и перестать потреблять ресурсы. Когда мы подключились к контейнеру и остановили задачу комбинацией Ctrl-C завершилась и работа контейнера. Контейнер остановился т.к. его задача была "выполнена". 

Шаг 6:

![Задача 3, пункт 6](https://github.com/user-attachments/assets/888f2a66-c866-4a9d-8fa0-2c9a511a5460)


 Шаг 7:

![Задача 3, пункт 7](https://github.com/user-attachments/assets/f8cc1069-5fab-408e-96dc-7a7487d12bbd)


Шаги 8-12:

![Задача 3, пункты 8-12](https://github.com/user-attachments/assets/3e7ff3da-9a98-48fa-83b7-1a1f4c85f31c)

Пункт 10: в пунтке 7 мы сменили порт который будет слушать nginx, это не тот порт который мы видим в выводе по команде "docker port custom-nginx-t2"

Задача №4.

![Задача 4](https://github.com/user-attachments/assets/f717ef26-d5e7-4cae-b87c-4197af54bea4)

Задача №5.

Шаг 1:

![Задача 5-1](https://github.com/user-attachments/assets/aa580b39-952c-4c34-91a0-226e84b3016a)

При запуске docker compose up -d выводятся 2 warning-а:


WARN[0000] Found multiple config files with supported names: /tmp/netology/docker/task5/compose.yaml, /tmp/netology/docker/task5/docker-compose.yaml

WARN[0000] Using /tmp/netology/docker/task5/compose.yaml


Запускаться будет только compose.yaml в котором не указано, что необходимо использовать еще один файл. 

Шаг 2:


Это можно исправить добавив в файл compose.yaml строчки чтобы включить другой compose файл:


include:
  -/task5/docker-compose.yaml






После этого запустятся оба файла:

![Задача 5-2](https://github.com/user-attachments/assets/277032aa-15cf-4de5-93d2-36e8ce6960f0)


Шаг 3:

![Задача 5, пункт 3](https://github.com/user-attachments/assets/d4fc5c2a-bd17-4466-befa-801d96422a04)


Забыл сохранить скриншот и уже сделал clear в терминале, опишу что делал. 

1. Сначала создаем и тегируем наш образ:
    ```bash
    docker pull nginx
    docker tag nginx 127.0.0.1:5000/custom-nginx:latest
    ```
2. Заливаем образ в локальный реестр:
    ```bash
    docker push 127.0.0.1:5000/custom-nginx:latest
    ```


Шаг 4-6:

![portainer](https://github.com/user-attachments/assets/e6723424-7e57-4b90-828b-51e997555957)


Поскольку у меня виртуалка без GUI и браузера сделал ssh-туннель до своего ПК, ssh -L 9000:localhost:9000 root@94.131.120.58, после чего все настроил по инструкции. 

Шаг 7:

![Задача 5, пункт 7](https://github.com/user-attachments/assets/0f283c98-d92f-462b-8908-5e9db5673b7c)


Исправил ошибку `docker compose up -d`, отредактировав файл `docker-compose.yaml`:



    ```yaml
    version: "3"
    services:
      portainer:
        network_mode: host
        image: portainer/portainer-ce:latest
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock
      registry:
        image: registry:2
        ports:
          - "5000:5000"
    ```
