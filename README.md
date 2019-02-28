# DOCKER PHP ENVIRONMENT

TODO - создать окружение на базу docker-compose позволяющее развернуть проект со всеми необходимыми для разработки на php сервисами, 
слушающее 80 порт.

Базовый состав окружения:
- php 7.3 (обновлять по мере необходимости)
- MySQL 8
- nginx
- php-fpm (рассмотреть целесообразность подключения nginx unit)

Дальнейшее развитие
- memcached || redis || couchbase
- NoSQL хранилище
- Средства мониторинга

На перспективу
Интеграция с системами оркестрации, разворачивание проекта в облаке.


На данный момент готово 2 образа: php-fpm и nginx. Оба выгружены на docker.hub и открыты для скачивания.
Образ mysql 8 используется стандартный, настройки задаются непосредственно в docker-compose.yml. Пример файла docker-compose вместе с разделом для монтирования находится в директории app_example данного репозитория.

## Описание php-fpm образа

TODO

## Описание nginx образа

TODO

## Описание mysql образа

В docker-compose.yml указана следующая строка:
```yml
command: --default-authentication-plugin=mysql_native_password
```
Она должна вернуть изменившийся в mysql 8 режим аутентификации, чтобы при запуске приложения не возникала ошибка SQLSTATE[HY000] [2002] Connection refused. Однако на некоторых системах это по-прежнему возникает. Для того чтобы устранить ее после запуска необходимо сделать следующее:
Попасть в контейнер с помощью команды.
```bash
docker exec -it your_mysql_container_name bash
```
далее в консоль mysql:
```bash
mysql -u root -p
```
после чего ввести настроенный в docker-compose.yml пароль и запустить в консоли mysql такой скрипт:
```mysql
ALTER USER 'root'@'localhost'
  IDENTIFIED WITH mysql_native_password
  BY 'password';
```

TODO - починить это!!!
