* [На главную](README.md)
* [Назад: Установка docker и docker compose](docker.md)

## Информация
В примерах используется адрес сайта a-0.example.com [Подробнее](hostname.md)

# Установка и первоначальная настройка nginx

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
rm -rf /tmp/debian-server-guide
```

Проверяем правильность настроек
```
sudo nginx -t
```

Добавляем nginx в автозапуск
```
sudo systemctl enable nginx
```

Запускаем nginx
```
sudo systemctl start nginx
```

Теперь по адресу a-0.example.com будет страница "Welcome to nginx!"

* [Далее: Настройка виртуальных хостов в nginx](nginx.virtual.hosts.md)