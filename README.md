
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose» Федоров Дмитрий

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
<details>
<summary>Ответ</summary>


![image](img/01.03.png)

</details>

- скачайте образ nginx:1.21.1;
<details>
<summary>Ответ</summary>


![image](img/01.04.png)

</details>

- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
<details>
<summary>Ответ</summary>


![image](img/01.05.png)

![image](img/01.05.01.png)

![image](img/01.05.02.png)

</details>

- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
<details>
<summary>Ответ</summary>


![image](img/01.06.png)

</details>

- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
<details>
<summary>Ответ</summary>


[Ссылка на репозиторий](https://hub.docker.com/repository/docker/demonmaf/custom-nginx/general)

</details>


## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
<details>
<summary>Ответ</summary>


![image](img/02.01.png)

</details>


2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
<details>
<summary>Ответ</summary>


![image](img/02.02.png)

</details>


3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
<details>
<summary>Ответ</summary>


![image](img/02.03.png)

</details>


4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
<details>
<summary>Ответ</summary>


![image](img/02.04.png)

![image](img/02.04.01.png)

</details>


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
<details>
<summary>Ответ</summary>


![image](img/03.01.png)

</details>

2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.

<details>
<summary>Ответ</summary>

![image](img/03.02.png)

Нажимаю Ctrl-C и выполняю команду docker ps -a Завершилась работа контейнера, т.к. Ctrl-C это сигнал завершения процесса, для фоновой работы процесса необходимо установить ключ -d и тогда контейнер продолжит работу, а мы отключимся от него.

</details>

4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
<details>
<summary>Ответ</summary>


![image](img/03.05.png)

</details>

7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
<details>
<summary>Ответ</summary>


![image](img/03.07.png)

</details>

8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
<details>
<summary>Ответ</summary>


![image](img/03.08.png)

</details>

9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
<details>
<summary>Ответ</summary>


![image](img/03.10.png)

</details>



11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
<details>
<summary>Ответ</summary>


![image](img/03.12.png)

</details>


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
<details>
<summary>Ответ</summary>


![image](img/04.02.png)

</details>

- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
<details>
<summary>Ответ</summary>


![image](img/04.03.png)

</details>

- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
<details>
<summary>Ответ</summary>


![image](img/04.04.png)

</details>

- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.
<details>
<summary>Ответ</summary>


![image](img/04.05.png)

</details>

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )
<details>
<summary>Ответ</summary>


![image](img/05.01.png)

![image](img/05.01.01.png)

Путь по умолчанию для файла Compose — compose.yaml(предпочтительно) или compose.yml, который находится в рабочем каталоге. Compose также поддерживает docker-compose.yamlи docker-compose.ymlдля обратной совместимости с более ранними версиями. Если существуют оба файла, Compose предпочитает канонический compose.yaml.

</details>


2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)
<details>
<summary>Ответ</summary>


![image](img/05.02.png)

![image](img/05.02.01.png)

</details>

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
<details>
<summary>Ответ</summary>


![image](img/05.03.png)

![image](img/05.03.01.png)

</details>

4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
<details>
<summary>Ответ</summary>


![image](img/05.04.png)

</details>

5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
<details>
<summary>Ответ</summary>


![image](img/05.05.png)

![image](img/05.05.01.png)

![image](img/05.05.02.png)


</details>

6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
<details>
<summary>Ответ</summary>


![image](img/05.06.png)

![image](img/05.06.01.png)

</details>


7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.
<details>
<summary>Ответ</summary>


![image](img/05.07.png)

![image](img/05.07.01.png)

![image](img/05.07.02.png)

![image](img/05.07.03.png)


</details>


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
