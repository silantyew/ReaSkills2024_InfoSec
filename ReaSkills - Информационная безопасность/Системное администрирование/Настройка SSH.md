# Настройка SSH
Создать выделенного пользователя (_sshuser_):

```text-plain
sudo useradd sshuser -s /bin/bash -p P@ssw0rd
```

Разрешить подключении по SSH конкретному пользователю(-ям):

_sudo nano /etc/ssh/sshd\_config_

```text-plain
AllowUsers sshuser
PermitRootLogin no
```

Ограничить максимальное количество активных сессий:

_sudo nano /etc/ssh/sshd\_config_

```text-plain
MaxSessions 4
```