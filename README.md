# DockerHW
Домашнее задание на тему "Работа с Docker и Docker Compose"
Установите Docker на хост машину https://docs.docker.com/engine/install/ubuntu/
Установите Docker Compose - как плагин, или как отдельное приложение
Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)
Определите разницу между контейнером и образом
Вывод опишите в домашнем задании.
Ответьте на вопрос: Можно ли в контейнере собрать ядро?
Для начала надо установить Docker и Docker Compose:
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
После чего проверяем установку Docker и Docker Compose:
sudo docker run hello-world
docker compose version
После чего создаём отдельную директорию:
mkdir dockernginx
cd dockernginx
Создаём файл index.html и dockerfile, прописывая нужные нам параметры и расписывая нашу кастомную страницу:
nano index.html
nano dockerfile
Производим сборку нашего образа:
docker build -t user_dockerhub/my-nginx:1.0
Запускаем наш контейнер:
docker run -d -p 8080:80 --name nginx-container user_dockerhub/my-nginx:1.0
Проверям работу:
curl http://localhost:8080
Для просмотра логов и остановки:
docker logs my-nginx-container
docker stop my-nginx-container
В конце не забываем залогиниться на DockerHub, залогиниться и закинуть образ:
docker login
docker push user_dockerhub/my-nginx
Ответы на вопроы:

Отличие образа от контейнера заключается в принипе работы:
Контейнер представляют изолированную среду на основе образа ОС, использующиеся как правило для изолирования процессов во время работы.
В сравнении с ВМ с образом занимает сильно меньше мощностей и места. Контейнер работает используя ядро хоста, т.е. контейнер сам по себе существовать не может.
Образ в ВМ может работать сам, без какой либо среды (ОС и есть среда). Он может работать когда угодно и зависит от своих собственных ресурсов.
В контейнере можно собрать ядро, но нельзя использовать, т.к. контейнер работает от ядра хоста. Можно с контейенра перекинуть на хост.
После этого хост сможет использовать собранное ядро.
