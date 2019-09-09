# koxx009_microservices

## Домашние задания
### Задание к занятию:
------------
#### 15. "Docker контейнеры. Docker под капотом"
В ходе задания были рассмотрены такие инструменты как: "docker", "docker-machine"

Послезные команды Docker:
1. Показать образы:
`docker images`

2. Показать активные контейнеры:
`docker ps`

3. Показать все контейнеры:
`docker ps -a`
или в отформатированном виде:
`docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Names}}"`

4. Запуск контейнеров:
 * `docker run $image_name` - создать, запустить и подключиться к контейнеру (при наличии опции -i)
 * `docker create $image_name` - создать контейнер но не запускать его
 * `docker start $u_container_id` - запустить контейнер
 * `docker attach $u_container_id` - подключиться к контейнеру (ENTER)

5. Остановка контейнера:
 * `docker stop $u_container_id` - посылает SIGTERM, и через 10 секунд(настраивается) посылает SIGKILL
 * `docker kill $u_container_id` - сразу посылает SIGKILL

6. Просмотр параметров занимаемого дискового пространства:
`docker system df`

7. Удаление:
 * `docker rm $u_container_id` - удаляет контейнер, можно добавить флаг -f, чтобы удалялся работающий container(будет послан sigkill)
 * `docker rmi $image_name` - удаляет образ


Также была настроена интеграция с GCP при помощи "docker-machine". Теперь можно разворачивать докер-контейнеры напряму в облаке.

Итогом задания стало создание своего докер контейнера "otus-reddit" и загрузка его в docker hub.


