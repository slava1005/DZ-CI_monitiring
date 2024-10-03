## 1.
```
Вас пригласили настроить мониторинг на проект. На онбординге вам рассказали,
что проект представляет из себя платформу для вычислений с выдачей текстовых отчетов,
которые сохраняются на диск. Взаимодействие с платформой осуществляется по протоколу http.
Также вам отметили, что вычисления загружают ЦПУ.
Какой минимальный набор метрик вы выведите в мониторинг и почему?
```

### Ответ
```
Нагрузка на процессор
Нагрузка на диск (скорость записи/чтения, индексные дескрипторы inodes)
Мониторинг API (доступность, ошибки)
Мониторинг RAM
```
## 2.
```
Менеджер продукта посмотрев на ваши метрики сказал, что ему непонятно что такое RAM/inodes/CPUla.
Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и
какое качество обслуживания. Что вы можете ему предложить?
```
### Ответ
```
RAM - утилизация оперативной памяти; inodes - кол-во индексных дескрипторов, на каждый файл используется 1 inode.
 Если будет большое кол-во лекгковесных файлов-отчётов, то в какой-то момент inodes могут закончиться,
при этом на диске может оставаться свободное пространство, но файлы не смогут создаваться; CPUia - показатель
средней загрузки CPU за промежуток времени.

Конечно, такие технические детали больше помогут админам найти проблемные и "узкие" места. Менеджерам и клиентам
важны не технические показатели, а что они получат в результате работы сервиса. Для определения качества обслуживания и
 выполнения обязанностей перед клиентом необходимо определить целевые показатели производительности. Для данного можно
выделить следующие показатели:

Допустимое время недоступности сервиса (единоразовое и/или допустимое время недоступности в определённый промежуток времени)
Допустимый процент ошибок при подготовке отчётов
Среднее время ожидания ответа от сервиса при подготовке отчёта
```
## 3.
```
Вашей DevOps команде в этом году не выделили финансирование на построение системы сбора логов.
Разработчики в свою очередь хотят видеть все ошибки, которые выдают их приложения. Какое решение
вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?
```
### Ответ
```
Можно настроить open source решение для мониторинга, например, zabbix, prometeus,
elastic stack, TICK stack и т.д.
```
## 4.
```
Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA=99% по http кодам ответов.
Вычисляете этот параметр по следующей формуле: summ_2xx_requests/summ_all_requests.
Данный параметр не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?
```
### Ответ
```
В формуле учтены только 2xx статус коды. Http запрос может завершаться с другими
"не ошибочными" кодами, которые тоже необходимо учесть:

Информационные 100 - 199
Перенаправления 300 - 399
```
## 5.
```
Опишите основные плюсы и минусы pull и push систем мониторинга.
```
### Ответ
```
push-модель

Плюсы:

упрощение репликации данных в разные системы мониторинга или их резервные копии
более гибкая настройка отправки пакетов данных с метриками
UDP — это менее затратный способ передачи данных, из-за чего может возрасти
производительность сбора метрик

Минусы:

UDP не гарантирует доставку данных
Необходимость проверки подлинности данных
Необходимость настройки агента
pull-модель

Плюсы:

легче контролировать подлинность данных
можно настроить единый proxy server до всех агентов с TLS
упрощённая отладка получения данных с агентов

Минусы:

Необходимость настройки узлов мониторинга
```
## 6.
```
Какие из ниже перечисленных систем относятся к push модели, а какие к pull?
А может есть гибридные?

Prometheus - pull

TICK - push

Zabbix - pull/push

VictoriaMetrics - pull/push

Nagios - push
```

##7.
Склонируйте себе репозиторий и запустите TICK-стэк, используя технологии docker и docker-compose. 
В виде решения на это упражнение приведите скриншот веб-интерфейса ПО chronograf (http://localhost:8888).

Прошу помощи!
Склонировал, запускаю командой ./sandbox up
```
slava@User-PC:~/sysmon10_2/sandbox$ sudo ./sandbox up
Using latest, stable releases
Spinning up Docker Images...
If this is your first time starting sandbox this might take a minute...
Building influxdb
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  4.096kB
Step 1/2 : ARG INFLUXDB_TAG
Step 2/2 : FROM influxdb:$INFLUXDB_TAG
 ---> 99bbc70e243d
Successfully built 99bbc70e243d
Successfully tagged influxdb:latest
Building telegraf
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  4.096kB
Step 1/2 : ARG TELEGRAF_TAG
Step 2/2 : FROM telegraf:$TELEGRAF_TAG
 ---> eaf8546f179e
Successfully built eaf8546f179e
Successfully tagged telegraf:latest
Building kapacitor
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  4.096kB
Step 1/2 : ARG KAPACITOR_TAG
Step 2/2 : FROM kapacitor:$KAPACITOR_TAG
 ---> 477ca5811095
Successfully built 477ca5811095
Successfully tagged kapacitor:latest
Building chronograf
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  6.144kB
Step 1/4 : ARG CHRONOGRAF_TAG
Step 2/4 : FROM chronograf:$CHRONOGRAF_TAG
 ---> a4e22bd1094c
Step 3/4 : ADD ./sandbox.src ./usr/share/chronograf/resources/
 ---> Using cache
 ---> 590e7bd4ad00
Step 4/4 : ADD ./sandbox-kapa.kap ./usr/share/chronograf/resources/
 ---> Using cache
 ---> 17738113de82
Successfully built 17738113de82
Successfully tagged chrono_config:latest
Building documentation
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  12.95MB
Step 1/6 : FROM alpine:3.12
 ---> 24c8ece58a1a
Step 2/6 : EXPOSE 3010:3000
 ---> Using cache
 ---> 0bf3fb038850
Step 3/6 : RUN mkdir -p /documentation
 ---> Using cache
 ---> 32a8e7df5119
Step 4/6 : COPY builds/documentation /documentation/
 ---> Using cache
 ---> 52e79cc189d5
Step 5/6 : COPY static/ /documentation/static
 ---> Using cache
 ---> 20aaaa8da30c
Step 6/6 : CMD ["/documentation/documentation", "-filePath", "/documentation/"]
 ---> Using cache
 ---> 407132d5d082
Successfully built 407132d5d082
Successfully tagged sandbox_documentation:latest
sandbox_documentation_1 is up-to-date
sandbox_influxdb_1 is up-to-date
Starting sandbox_kapacitor_1 ... 
Starting sandbox_kapacitor_1 ... done
Starting sandbox_chronograf_1 ... done
Opening tabs in browser...
```
В браузере не поднимается страница Chronograf http://localhost:8888/. 
В тоже время страница с документацией http://localhost:3010/ поднимается отлично 
Очень прошу помощи...

Прикрепляю логи контейнеров, от которых зависит хронограф, и логи контейнера самого хронографа
```
slava@User-PC:~/sysmon10_2/sandbox$ sudo docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                                                                                                                             NAMES
496acb325050   telegraf                "/entrypoint.sh tele…"   32 seconds ago   Up 30 seconds   8092/udp, 8125/udp, 8094/tcp                                                                                                      sandbox_telegraf_1
48bd130c9e94   influxdb                "/entrypoint.sh infl…"   33 seconds ago   Up 32 seconds   0.0.0.0:8082->8082/tcp, :::8082->8082/tcp, 0.0.0.0:8086->8086/tcp, :::8086->8086/tcp, 0.0.0.0:8089->8089/udp, :::8089->8089/udp   sandbox_influxdb_1
b5a3e604b851   sandbox_documentation   "/documentation/docu…"   33 seconds ago   Up 32 seconds   0.0.0.0:3010->3000/tcp, :::3010->3000/tcp sandbox_documentation_1

slava@User-PC:~/sysmon10_2/sandbox$ sudo docker logs 496acb325050
2024-10-03T16:55:54Z I! Loading config: /etc/telegraf/telegraf.conf
2024-10-03T16:55:54Z W! DeprecationWarning: Option "container_names" of plugin "inputs.docker" deprecated since version 1.4.0 and will be removed in 1.35.0: use 'container_name_include' instead
2024-10-03T16:55:54Z W! DeprecationWarning: Option "perdevice" of plugin "inputs.docker" deprecated since version 1.18.0 and will be removed in 1.35.0: use 'perdevice_include' instead
2024-10-03T16:55:54Z I! Starting Telegraf 1.32.0 brought to you by InfluxData the makers of InfluxDB
2024-10-03T16:55:54Z I! Available plugins: 235 inputs, 9 aggregators, 32 processors, 26 parsers, 62 outputs, 6 secret-stores
2024-10-03T16:55:54Z I! Loaded inputs: cpu docker influxdb syslog system
2024-10-03T16:55:54Z I! Loaded aggregators:
2024-10-03T16:55:54Z I! Loaded processors:
2024-10-03T16:55:54Z I! Loaded secretstores:
2024-10-03T16:55:54Z I! Loaded outputs: influxdb
2024-10-03T16:55:54Z I! Tags enabled: host=telegraf-getting-started
2024-10-03T16:55:54Z W! Deprecated inputs: 0 and 2 options
2024-10-03T16:55:54Z I! [agent] Config: Interval:5s, Quiet:false, Hostname:"telegraf-getting-started", Flush Interval:5s
2024-10-03T16:55:54Z W! [outputs.influxdb] When writing to [http://influxdb:8086]: database "telegraf" creation failed: Post "http://influxdb:8086/query": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:55:54Z I! [inputs.syslog] Listening on tcp://127.0.0.1:6514
2024-10-03T16:55:55Z E! [inputs.influxdb] Error in plugin: Get "http://influxdb:8086/debug/vars": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:55:59Z E! [outputs.influxdb] When writing to [http://influxdb:8086]: failed doing req: Post "http://influxdb:8086/write?consistency=any&db=telegraf": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:55:59Z E! [agent] Error writing to outputs.influxdb: could not write any address
2024-10-03T16:56:00Z E! [inputs.influxdb] Error in plugin: Get "http://influxdb:8086/debug/vars": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:56:04Z E! [outputs.influxdb] When writing to [http://influxdb:8086]: failed doing req: Post "http://influxdb:8086/write?consistency=any&db=telegraf": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:56:04Z E! [agent] Error writing to outputs.influxdb: could not write any address
2024-10-03T16:56:05Z E! [inputs.influxdb] Error in plugin: Get "http://influxdb:8086/debug/vars": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:56:09Z E! [outputs.influxdb] When writing to [http://influxdb:8086]: failed doing req: Post "http://influxdb:8086/write?consistency=any&db=telegraf": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:56:09Z E! [agent] Error writing to outputs.influxdb: could not write any address
2024-10-03T16:56:10Z E! [inputs.influxdb] Error in plugin: Get "http://influxdb:8086/debug/vars": dial tcp 172.23.0.3:8086: connect: connection refused
2024-10-03T16:56:14Z E! [outputs.influxdb] When writing to [http://influxdb:8086]: 404 Not Found: database not found: "telegraf"
2024-10-03T16:56:14Z E! [agent] Error writing to outputs.influxdb: database created; retry write

slava@User-PC:~/sysmon10_2/sandbox$ sudo docker logs 48bd130c9e94
ts=2024-10-03T16:55:51.012960Z lvl=info msg="InfluxDB starting" log_id=0s0pnHu0000 version=1.8.10 branch=1.8 commit=688e697c51fd
ts=2024-10-03T16:55:51.013060Z lvl=info msg="Go runtime" log_id=0s0pnHu0000 version=go1.13.8 maxprocs=4
ts=2024-10-03T16:56:12.588881Z lvl=info msg="Using data dir" log_id=0s0pnHu0000 service=store path=/var/lib/influxdb/data
ts=2024-10-03T16:56:12.589651Z lvl=info msg="Compaction settings" log_id=0s0pnHu0000 service=store max_concurrent_compactions=2 throughput_bytes_per_second=50331648 throughput_bytes_per_second_burst=50331648
ts=2024-10-03T16:56:12.589716Z lvl=info msg="Open store (start)" log_id=0s0pnHu0000 service=store trace_id=0s0pobBG000 op_name=tsdb_open op_event=start
ts=2024-10-03T16:56:12.589816Z lvl=info msg="Open store (end)" log_id=0s0pnHu0000 service=store trace_id=0s0pobBG000 op_name=tsdb_open op_event=end op_elapsed=0.105ms
ts=2024-10-03T16:56:12.594608Z lvl=info msg="Opened service" log_id=0s0pnHu0000 service=subscriber
ts=2024-10-03T16:56:12.594905Z lvl=info msg="Starting monitor service" log_id=0s0pnHu0000 service=monitor
ts=2024-10-03T16:56:12.594935Z lvl=info msg="Registered diagnostics client" log_id=0s0pnHu0000 service=monitor name=build
ts=2024-10-03T16:56:12.595003Z lvl=info msg="Registered diagnostics client" log_id=0s0pnHu0000 service=monitor name=runtime
ts=2024-10-03T16:56:12.595025Z lvl=info msg="Registered diagnostics client" log_id=0s0pnHu0000 service=monitor name=network
ts=2024-10-03T16:56:12.595037Z lvl=info msg="Registered diagnostics client" log_id=0s0pnHu0000 service=monitor name=system
ts=2024-10-03T16:56:12.596981Z lvl=info msg="Starting precreation service" log_id=0s0pnHu0000 service=shard-precreation check_interval=10m advance_period=30m
ts=2024-10-03T16:56:12.597037Z lvl=info msg="Starting snapshot service" log_id=0s0pnHu0000 service=snapshot
ts=2024-10-03T16:56:12.597449Z lvl=info msg="Starting continuous query service" log_id=0s0pnHu0000 service=continuous_querier
ts=2024-10-03T16:56:12.597474Z lvl=info msg="Starting HTTP service" log_id=0s0pnHu0000 service=httpd authentication=false
ts=2024-10-03T16:56:12.597488Z lvl=info msg="opened HTTP access log" log_id=0s0pnHu0000 service=httpd path=stderr
ts=2024-10-03T16:56:12.597827Z lvl=info msg="Listening on HTTP" log_id=0s0pnHu0000 service=httpd addr=[::]:8086 https=false
ts=2024-10-03T16:56:12.600471Z lvl=info msg="Starting retention policy enforcement service" log_id=0s0pnHu0000 service=retention check_interval=30m
ts=2024-10-03T16:56:12.601235Z lvl=info msg="Started listening on UDP" log_id=0s0pnHu0000 service=udp addr=:8089
ts=2024-10-03T16:56:12.602419Z lvl=info msg="Listening for signals" log_id=0s0pnHu0000
ts=2024-10-03T16:56:12.597252Z lvl=info msg="Storing statistics" log_id=0s0pnHu0000 service=monitor db_instance=_internal db_rp=monitor interval=10s
ts=2024-10-03T16:56:12.606055Z lvl=info msg="Sending usage statistics to usage.influxdata.com" log_id=0s0pnHu0000
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:14 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 404 45 "-" "Telegraf/1.32.0 Go/1.23.0" 66a92382-81a8-11ef-8001-0242ac170003 496
ts=2024-10-03T16:56:14.891276Z lvl=info msg="Executing query" log_id=0s0pnHu0000 service=query query="CREATE DATABASE telegraf"
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:14 +0000] "POST /query HTTP/1.1 {'q': 'CREATE DATABASE "telegraf"'}" 200 57 "-" "Telegraf/1.32.0 Go/1.23.0" 66ac74fa-81a8-11ef-8002-0242ac170003 17685
ts=2024-10-03T16:56:20.029014Z lvl=info msg="index opened with 8 partitions" log_id=0s0pnHu0000 index=tsi
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:19 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 69a3dd02-81a8-11ef-8003-0242ac170003 256349
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:24 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 6c9ef05d-81a8-11ef-8004-0242ac170003 74726
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:29 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 6f99fa97-81a8-11ef-8005-0242ac170003 12288
ts=2024-10-03T16:56:30.163120Z lvl=info msg="index opened with 8 partitions" log_id=0s0pnHu0000 index=tsi
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:34 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 72951339-81a8-11ef-8006-0242ac170003 225883
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:39 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 759025d1-81a8-11ef-8007-0242ac170003 22925
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:44 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 788b2506-81a8-11ef-8008-0242ac170003 23009
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:49 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 7b862712-81a8-11ef-8009-0242ac170003 12001
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:54 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 7e813e7e-81a8-11ef-800a-0242ac170003 38139
[httpd] 172.23.0.5 - - [03/Oct/2024:16:56:59 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 817c4376-81a8-11ef-800b-0242ac170003 28311
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:04 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 8477902c-81a8-11ef-800c-0242ac170003 56533
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:09 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 8772376b-81a8-11ef-800d-0242ac170003 17305
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:14 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 8a6ea5fd-81a8-11ef-800e-0242ac170003 49555
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:19 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 8d68712f-81a8-11ef-800f-0242ac170003 24736
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:24 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 90636529-81a8-11ef-8010-0242ac170003 25691
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:29 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 935e6959-81a8-11ef-8011-0242ac170003 24473
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:34 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 96597036-81a8-11ef-8012-0242ac170003 14985
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:39 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 99549a96-81a8-11ef-8013-0242ac170003 33068
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:44 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 9c4fbf24-81a8-11ef-8014-0242ac170003 35486
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:49 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" 9f4aceec-81a8-11ef-8015-0242ac170003 123312
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:54 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" a245fd30-81a8-11ef-8016-0242ac170003 28246
[httpd] 172.23.0.5 - - [03/Oct/2024:16:57:59 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" a540e7ec-81a8-11ef-8017-0242ac170003 26504
[httpd] 172.23.0.5 - - [03/Oct/2024:16:58:04 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" a83c0cb2-81a8-11ef-8018-0242ac170003 41055
[httpd] 172.23.0.5 - - [03/Oct/2024:16:58:09 +0000] "POST /write?consistency=any&db=telegraf HTTP/1.1 " 204 0 "-" "Telegraf/1.32.0 Go/1.23.0" ab36fd34-81a8-11ef-8019-0242ac170003 24312

slava@User-PC:~/sysmon10_2/sandbox$ sudo docker logs b5a3e604b851
2024/10/03 16:55:50 Files in path /documentation/
2024/10/03 16:55:50 Server started. Listening on port :3000
slava@User-PC:~/sysmon10_2/sandbox$
```
