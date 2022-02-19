[На главную](README.md)

# Создание sudo пользователя
Больше не будем использовать пользователя root, создадим своего

```
apt install sudo
```

```
adduser perun
```
Отвечаем на вопросы, после чего должна появится директория /home/perun

Проверим
```
ls /home
```

Добавляем пользователя в группу sudo
```
usermod -aG sudo perun
```

Проверяем есть ли запись sudo perun
```
getent group sudo
```

Выходим с сервера и заходим новым пользователем
```
exit
```

```
ssh-copy-id perun@a-0.example.com
ssh perun@a-0.example.com
```

[Настройка подсветки терминала](bashrc.md)