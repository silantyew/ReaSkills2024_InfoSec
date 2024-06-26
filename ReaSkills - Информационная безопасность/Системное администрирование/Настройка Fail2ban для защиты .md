# Настройка Fail2ban для защиты SSH
Fail2ban — простой в использовании локальный сервис, который отслеживает log–файлы запущенных программ, и на основании различных условий блокирует по IP найденных нарушителей.

Программа умеет бороться с различными атаками на все популярные \*NIX–сервисы, такие как Apache, Nginx, ProFTPD, vsftpd, Exim, Postfix, named, и т.д.

Но в первую очередь Fail2ban известен благодаря готовности «из коробки» к защите SSH–сервера от атак типа «bruteforce», то есть к защите SSH от перебора паролей.

#### Установка Fail2ban

Готовые пакеты Fail2ban можно найти в официальных репозиториях всех популярных Linux дистрибутивов.

Установка Fail2ban на Debian/Ubuntu:

`apt-get install fail2ban`

Установка Fail2ban на CentOS/Fedora/RHEL:

`yum install fail2ban`

#### Конфигурация Fail2ban

На данном этапе Fail2ban уже готов к работе, базовая защита SSH сервера от перебора паролей будет включена по умолчанию. Но лучше всё-же внести некоторые изменения следуя рекомендациям ниже.

У программы два основных файла конфигурации:

1.  `/etc/fail2ban/fail2ban.conf` — отвечает за настройки запуска процесса Fail2ban.
2.  `/etc/fail2ban/jail.conf` — содержит настройки защиты конкретных сервисов, в том числе sshd.

Файл `jail.conf` поделён на секции, так называемые «изоляторы» (jails), каждая секция отвечает за определённый сервис и тип атаки:

```text-plain
[DEFAULT]
ignoreip = 127.0.0.1/8
bantime  = 600
maxretry = 3
banaction = iptables-multiport

[ssh]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 6
```

Параметры из секции `[DEFAULT]` применяются ко всем остальным секциям, если не будут переопределены.

Секция `[ssh]` отвечает за защиту SSH от повторяющихся неудачных попыток авторизации на SSH–сервере, проще говоря, «brute–force».

Подробнее по каждому из основных параметров файла jail.conf:

**ignoreip** — IP–адреса, которые не должны быть заблокированы. Можно задать список IP-адресов разделённых пробелами, маску подсети, или имя DNS–сервера.

**bantime** — время бана в секундах, по истечении которого IP–адрес удаляется из списка заблокированных.

**maxretry** — количество подозрительных совпадений, после которых применяется правило. В контексте `[ssh]` — это число неудавшихся попыток логина, после которых происходит блокировка.

**enabled** — значение `true` указывает что данный jail активен, `false` выключает действие изолятора.

**port** — указывает на каком порту или портах запущен целевой сервис. Стандартный порт SSH–сервера — `22`, или его буквенное наименование — `ssh`.

**filter** — имя фильтра с регулярными выражениями, по которым идёт поиск «подозрительных совпадений» в журналах сервиса. Фильтру `sshd` соответствует файл `/etc/fail2ban/filter.d/sshd.conf`.

**logpath** — путь к файлу журнала, который программа Fail2ban будет обрабатывать с помощью заданного ранее фильтра. Вся история удачных и неудачных входов в систему, в том числе и по SSH, по умолчанию записывается в log–файл `/var/log/auth.log`.

#### Рекомендации по настройке Fail2ban

Не рекомендуется оставлять параметр **ignoreip** со значением по умолчанию `127.0.0.1/8`, это создаёт очевидную угрозу в многопользовательских системах — если злоумышленник получил доступ хотя–бы к одному shell–аккаунту, то он имеет возможность беспрепятственно запустить bruteforce–программу для атаки на root или других пользователей прямо с этого–же сервера.

Новая опция **findtime** — определяет длительность интервала в секундах, за которое событие должно повториться определённое количество раз, после чего санкции вступят в силу. Если специально не определить этот параметр, то будет установлено значение по умолчанию равное 600 (10 минут). Проблема в том, что ботнеты, участвующие в «медленном брутфорсе», умеют обманывать стандартное значение. Иначе говоря, при **maxretry** равным 6, атакующий может проверить 5 паролей, затем выждать 10 минут, проверить ещё 5 паролей, повторять это снова и снова, и его IP забанен не будет. В целом, это не угроза, но всё же лучше банить таких ботов.

Прежде чем вносить изменения следуя рекомендациям, отметим, что не стоит редактировать основной файл настроек `jail.conf`, для этого предусмотрены файлы с расширением `*.local`, которые автоматически подключаются и имеют высший приоритет.

#### Просмотр статистики Fail2ban

`sudo fail2ban-client status sshd` - утилита fail2ban-client позволяет следить за "клетками" (jails)