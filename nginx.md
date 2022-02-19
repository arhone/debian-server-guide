[На главную](README.md)

# Установка и настройка nginx

Nginx обрабатывает огромное количество запросов.

Нет необходимости постоянно обновлять и перенастраивать nginx на сервере, 
обычно обновляются только настройки виртуальных хостов.

По этому nginx лучше ставить на хост системе, а не в docker контейнере.

Так и сделаем.

Устанавливаем nginx
```
sudo apt install nginx
```

Копируем файл настроек
```
cd /tmp
git clone https://github.com/arhone/debian-server-guide.git
sudo cp /tmp/debian-server-guide/config/nginx/nginx.conf /etc/nginx/nginx.conf
```