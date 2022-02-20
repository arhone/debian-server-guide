* [На главную](README.md)
* [Назад: Установка и настройка nginx](nginx.md)

## Информация
В примерах используется адрес сайта a-0.example.com [Подробнее](hostname.md)

# Настройка виртуальных хостов в nginx

В debian используется две директории для настройки доменов
1. /etc/nginx/sites-available - Директория с файлами настроек
2. /etc/nginx/sites-enabled - Директория с ссылками на файлы настроек из директории sites-available

Так сделано для удобства.

Можно отключить какой-то сайт просто удалив ссылку, не удалять сам файл.
Можно долго настраивать новый, не боясь что nginx его подключит раньше времени.

По умолчанию nginx создаёт default настройку со страницей "Welcome to nginx!"
Оригинал лежит в директории sites-available
```/etc/nginx/sites-available/default```

А в nginx он подключается с помощью ссылки на этот файл, которая лежит в директории
```/etc/nginx/sites-enabled/default```

Удалим ссылку на default
```
sudo rm /etc/nginx/sites-enabled/default 
sudo nginx -t
sudo nginx -s reload
```

Теперь у вас нет страницы с надписью "Welcome to nginx!"

Вернём её обратно
```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
sudo nginx -t
sudo nginx -s reload
```

На сайте снова надпись "Welcome to nginx!"

## Чистка мусора

На данный момент nginx показывает только demo страницу, которая нам больше не понадобится

Удалим ссылку на её настройку
```
sudo rm /etc/nginx/sites-enabled/default 
sudo nginx -t
sudo nginx -s reload
```

Все наши проекты будут лежать в директории /srv,
а demo страница nginx лежит по старинке в /var/www, которая нам больше не нужна

Удалим эту директорию
```
sudo rm -rf /var/www
```

## Добавление нового домена

...
