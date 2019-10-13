# koxx009_microservices

## Домашние задания
### Задание к занятию:
------------
#### 15. "Docker контейнеры. Docker под капотом"
В ходе задания были рассмотрены такие инструменты как: "docker", "docker-machine"

Полезные команды Docker:
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


----------

#### 16 "Docker образы. Микросервисы"

В ходе данного задания, была более подробно рассмотрена микросервисная структура приложений.
Была изучено приложение "hadolint". Она позволяет выполнить проверку Dockerfile на корректность и оптимальность.

Синтекс "hadolint":
`hadolint <PATH_TO_DOCKERFILE>`

Также было показаны способы взаимодействия различных контейнеров между собой, при помощь "--network" и "--network-alias".

----------

#### 17 "Сетевое взаимодействие Docker контейнеров. Docker Compose. Тестирование образов"

В ходе задания был рассмотрен "docker-compose" а также сетевое взаимодействие между контейнерами.

Для создания новой сети для docker необходимо выполнить следующую команду:
 - `docker network create ${NETWORK_NAME} --subnet=10.0.1.0/24`

По умолчанию при инициализации контейнера можно подключить только одну сеть. Например для контейнера comment:
 - `docker run -d --network=${NETWORK_NAME} --network-alias=comment ${USERNAME}/comment:1.0` 

Для подключение дополнительной сети, после инициализации контейнера необходимо выполнить команду:
 - `docker network connect ${NETWORK_NAME} ${CONTAINER_ID}`

Таким образом, можно отделить различные сервисы по сетям, тем самым обезопасить сервис, т.к. FRONT (UI) часть не видит BACK (БД).


Для упрощения процедуры разворачивания микросервисного сервиса, можно использовать docker-compose благодаря которому можно параметризировать процесс разворачивания контейнеров.
Для этого необходимо составить создать файл "docker-compose.yml", в котором будет указано какие контейнеры в какой последовательности необходимо разворачивать, а также указать все остальные параметры, такие как --network, --network-alias и т.п.

Docker-compose поддерживает переменные внутри файла "docker-compose.yml".
Для задания переменных используется стандартный linux инструмент - export $VAR_NAME. Но вручную вводить переменные не удобно, по этому в docker-compose есть поддержка описания переменных в файл - ".env"

Внутри файла `.env` переменные указываются в формате:
 * "ИМЯ_ПЕРЕМЕННОЙ=ЗНАЧЕНИЕ"

Например для замены стандартного имени проекта можно использовать переменную:
 - `COMPOSE_PROJECT_NAME=$PROJECT_NAME`

При этом в файле "docker-compose.yml", переменные указываются в формате - `${COMPOSE_PROJECT_NAME}`

----------

#### 19 "Устройство Gitlab CI. Построение процесса непрерывной интеграции"

В ходе задания был развернут Gitlab-ce из контейнера через docker-machine.
Также был рассмотрен CI/CD на основе gitlab.

Также было рассмотренно использование git remote с двумя репозиториями одновременно.

----------

#### 20 "Введение в мониторинг. Модели и принципы работы систем мониторинга"

В ходе задания был рассмотрен инструмент Prometheus, благодаря которому можно собирать метрику различных приложений.

Также был рассмотрен инструмент "node_exporter" благодаря которому, появляется возможность собирать метрику с тех приложение в которых изначально нет такой возможности.

----------

#### 21 "Мониторинг приложения и инфраструктуры"

В ходе задания были рассмотрены:
 - продвинутые функии Prometheus
 - интеграция Prometheus с Grafana
 - интеграция Prometheus с Alertmanager

В Grafana были настроены 2 дашборда:
 - UI_Service_Monitoring
 - Business_Logic_Monitoring

Оба дашборда сохранены в формате JSON в соответствующих файлах в каталоге `./monitoring/grafana/dashboards`

Также в ходе интеграции Prometheus с Alertmanager была настроена нотификация в персональный канал Slack, с помощью плагина "Incoming Webhook"

Итоговые образы докер-контейнеров залиты в персональный репозиторий Docker Hub. Ниже ссылки на проекты в Docker Hub:
 - https://hub.docker.com/r/koxx009/alertmanager
 - https://hub.docker.com/r/koxx009/prometheus
 - https://hub.docker.com/r/koxx009/post
 - https://hub.docker.com/r/koxx009/comment
 - https://hub.docker.com/r/koxx009/ui

----------

#### 23 "Применение системы логирования в инфраструктуре на основе Docker."

В ходе задания были рассмотрены следующие средства логирования:
 - ELK (EFK) stack
 - Zipkin

Были рассмотрены 3 способа логирования:
 - Форматированные логи (в формате json) + разделение по объектам массива, при помощи параметра "format json"
 - Неформатированные логи - с разделением при помощи регулярных выражений (regexp)
 - Неформатированные логи - с grok-шаблона

Также был рассмотрен инструмент для сбора трассировок:
 - Zipkin

----------


