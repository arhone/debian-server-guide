[На главную](README.md)

# Установка docker и docker compose на debian 10

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
Создаём директорию docker для текущего пользователя
```
mkdir -p ~/.docker/cli-plugins/
```

Проверяем последнюю версию [docker compose](https://github.com/docker/compose/releases)

На момент написания это 2.2.3

Скачиваем последнюю версию
```
curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
```

Устанавливаем права на доступ к исполняемому файлу
```
chmod +x ~/.docker/cli-plugins/docker-compose
```

Проверим версию compose
```
docker compose version
```