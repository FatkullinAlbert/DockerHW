# DockerHW
Домашнее задание на тему "Работа с Docker и Docker Compose"
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
