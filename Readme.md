## Фильтрация трафика - firewalld, iptables

Vagrantfile - вагрант файл

Для открытия SSH необходимо выполнить команду knock.sh 192.168.255.1 7000 8000 9000
Для закрытия SSH необходимо выполнить команду knock.sh 192.168.255.1 6000 7000 8000

Лог с inetRouter после открытия порта:

```
[root@inetRouter var]# cat /var/log/knockd.log 
[2023-09-19 12:39] starting up, listening on eth0
[2023-09-19 12:55] waiting for child processes...
[2023-09-19 12:55] shutting down
[2023-09-19 12:56] starting up, listening on eth0
[2023-09-19 13:00] waiting for child processes...
[2023-09-19 13:00] shutting down
[2023-09-19 13:00] starting up, listening on eth1
[2023-09-19 13:00] 192.168.255.2: openSSH: Stage 1
[2023-09-19 13:00] 192.168.255.2: openSSH: Stage 2
[2023-09-19 13:00] 192.168.255.2: openSSH: Stage 3
[2023-09-19 13:00] 192.168.255.2: openSSH: OPEN SESAME
[2023-09-19 13:00] openSSH: running command: /sbin/iptables -I INPUT -s 192.168.255.2 -p tcp --dport 22 -j ACCEPT
```

Tcpdump проброса порта на centralRouter c 80 порта 192.168.255.2 на 192.168.255.3:8080 Дамп _[tcpdump](tcpdump.png)_
