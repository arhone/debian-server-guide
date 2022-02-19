* [На главную](README.md)
* [Назад: Настройка подсветки терминала](bashrc.md)

# Установка docker и docker compose v2 на debian 10

## Информация
В примерах используется имя пользователя "perun" [Подробнее](user.md)

## Установка docker engine

Установим необходимые пакеты
```
sudo apt install ca-certificates
sudo apt install curl
sudo apt install gnupg
sudo apt install lsb-release
```

Добавим официальный GPG-ключ Docker:
```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Добавим стабильный репозиторий docker
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Обновим индекс пакетов
```
sudo apt update
```

Установим docker
```
sudo apt install docker-ce docker-ce-cli containerd.io
```

## Установка docker compose
Создаём директорию docker для всех пользователей
```
sudo mkdir -p /usr/local/lib/docker/cli-plugins
```

Проверяем последнюю версию [docker compose](https://github.com/docker/compose/releases)

На момент написания это 2.2.3

Скачиваем последнюю версию
```
sudo curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
```

Устанавливаем права на доступ к исполняемому файлу
```
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

Проверим версию compose
```
docker compose version
```

## Проверка сборки и запуска контейнера

Перейдём в директорию проектов
```
cd /srv
```

Создадим директорию docker-alpine и дадим права текущему пользователю
```
sudo mkdir docker-alpine
sudo chown $USER:$USER docker-alpine
```

Клонируем проект [docker-alpine](https://github.com/arhone/docker-alpine) в директорию docker-alpine
```
git clone https://github.com/arhone/docker-alpine.git /srv/docker-alpine
```

Перейдём в директорию docker-alpine, соберём и запустим контейнер
```
cd /srv/docker-alpine
cp .env.example .env
sudo docker compose -f docker-compose.yml up -d --build --remove-orphans
```

Проверим есть ли такой контейнер и зайдём в него
```
sudo docker container ls
sudo docker exec -it docker-alpine-01 /bin/sh
```

## Разрешить команду sudo docker без запроса пароля 
```
sudo visudo
```
Добавить запись (perun - это имя пользователя для примера)
```
perun ALL=NOPASSWD: /usr/bin/docker
```

* [Далее: Установка и настройка nginx](nginx.md)