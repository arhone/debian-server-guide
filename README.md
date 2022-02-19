# debian-server-configuring
Инструкция по настройке сервера на OS Linux Debian

# Настройка

## Название нового сервера
Называем сервер по правилу таблицы виртуального дата центра в нижнем регистре.

Где столбы это виртуальные стойки, а поля, это позиция сервера в стойке (стойка-позиция).

Например a-0.example.com или b1-aa.example.com отображены на таблице как "___"

|     | A   | B   | C   | D   | ... | A0  | A1  | AA  | AB  | B0  | B1  | ... | ABC |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|----|
| 0   | ___ |
| 1   |
| 2   |
| ... |
| 9   |
| A   |
| B   |
| ... |
| A0  |
| ... |
| AA  |     |     |     |     |     |     |     |     |     |     | ___ |     |    |
| AB  |

Прописываем выбранный технический адрес сервера в DNS (далее для примера это будет a-0.example.com)

## Подключение к серверу по ssh
Добавляем свой ключ, что бы заходить по ключу, а не по паролю
```
ssh-copy-id root@a-0.example.com
```
Вводим пароль

Дальше входим уже без пароля
```
ssh root@a-0.example.com
```

## Обновляем до актуального состояния
```
apt update -y
```
```
apt upgrade -y
```

## Смена hostname на сервере
Смотрим какой сейчас hostname
```
hostname
```

Меняем на a-0.example.com
```
hostnamectl set-hostname a-0.example.com
```

Выходим с сервера командой exit и заходим опять. Видим root@a-0

## Создание пользователя с sudo
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

## Настройка .bashrc
Что бы не путаться находитесь ли вы в терминале на локальной машине или на сервере, нужно поменять подсветку. 
Сделаем hostname красным цветом.

Так же новая подсветка будет показывать текущую git ветку каталога, в котором вы находитесь, по этому сразу поставим git.

```
sudo apt install git
```

Открываем файл .bashrc в редакторе nano 
```
nano /home/perun/.bashrc
```

Находим блок и закомментируем его вот так
```
#if [ "$color_prompt" = yes ]; then
#    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
#else
#    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
#fi
```

Над старым блоком вставим новый
```
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/[\1]/"
}
if [ "$color_prompt" = yes ]; then
     PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u\[\033[00m\]\[\033[01;31m\]@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\] \[\033[01;33m\]$(parse_git_branch)\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
```

Сохраняем.

Выходим и снова заходим на сервер и видим новую подсветку.

Теперь проверим работу подсветки ветки git и заодно сохраним этот проект настройки в директории для проектов /srv

Создаём директорию debian-server-configuring
```
sudo mkdir /srv/debian-server-configuring 
```

Меняем владельца директории с root, на perun
```
sudo chown $USER:$USER /srv/debian-server-configuring/
```

Клонируем этот репозиторий в директорию debian-server-configuring
```
git clone https://github.com/arhone/debian-server-configuring.git /srv/debian-server-configuring/
```

Заходим в директорию проекта и видим ветку [main]
```
cd /srv/debian-server-configuring
```