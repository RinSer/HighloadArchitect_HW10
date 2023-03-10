## Инструкция для запуска:

1) Установить Docker и python 3.8
2.1) Выполнить в текущей директории CLI команду `docker build --build-arg cnf=node1.cnf -t ubuntu:galera-node1 ./` для создания образа мастер ноды кластера.
2.2) Выполнить в текущей директории CLI команду `docker build --build-arg cnf=node2.cnf -t ubuntu:galera-node2 ./slave` для создания образа мастер ноды кластера.
3.1) Создаем сеть в докере: `docker network create --subnet=192.168.0.0/24 hw3`.
3.2) Запуск первой ноды кластера: `docker run -i -d --name Node1 --hostname node1 --network="hw3" --ip=192.168.0.101 -p 3301:3306 -v /var/container_data/mysql:/var/lib/mysql ubuntu:galera-node1`.
3.3) Добавляем пользователя приложения и БД:
`mysql -u root -p -e " CREATE USER 'haproxy_check'@'%'; CREATE DATABASE social_network; CREATE USER 'test'@'%' IDENTIFIED BY 'test'; GRANT ALL ON *.* to test@'%'; FLUSH PRIVILEGES;"`
`mysql -u root -p -e "GRANT ALL ON *.* to test@'192.168.0.1' IDENTIFIED BY 'test'; FLUSH PRIVILEGES;"`
4) Выполнить в текущей директории CLI команду `pip install -r requirements.txt` для установки зависимостей приложения.
5) Выполнить в текущей директории CLI команду `py -3 -m flask --app app run` для локального запуска приложения (порт 5000).

Обновить requirements.txt: `py -3 -m  pipreqs.pipreqs --force`