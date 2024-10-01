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
