[На главную](README.md)

# Информация
В примерах используется имя пользователя "perun" [Подробнее](user.md)

# Настройка .bashrc
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

Создаём директорию debian-server-guide
```
sudo mkdir /srv/debian-server-guide 
```

Меняем владельца директории с root, на perun
```
sudo chown $USER:$USER /srv/debian-server-guide/
```

Клонируем этот репозиторий в директорию debian-server-guide
```
git clone https://github.com/arhone/debian-server-guide.git /srv/debian-server-guide/
```

Заходим в директорию проекта и видим ветку [main]
```
cd /srv/debian-server-guide
```

[Установка docker и docker compose](docker.md)
