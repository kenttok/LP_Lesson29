# Мосты, туннели и VPN

# 1. Между двумя виртуалками поднять vpn в режимах tun/tap

а) ```iperf3 -c 10.10.10.1 -t 40 -i 5```
 - tun 
 
 ![Image_alt](https://github.com/kenttok/LP_Lesson34/blob/main/4.png)
 
 - tap

![Image_alt](https://github.com/kenttok/LP_Lesson34/blob/main/3.png)

В режиме ```tun``` отправлено больше данных.

Разница tun и tap режимов:

Функционируют на разных уровнях OSI. TAP эмулирует устройство Ethernet и действует на канальном уровне OSI, обрабатывая Ethernet-кадры. В то время как TUN работает на сетевом уровне OSI, манипулируя IP-пакетами. TAP применяется для создания сетевых мостов, а TUN – для маршрутизации.

# 2. RAS на базе OpenVPN

На стороне сервера проверяем что порт 1207 слушается

 ![Image_alt](https://github.com/kenttok/LP_Lesson34/blob/main/1.png)

На стороне клиента выполняем команды:

```
vagrant ssh -c 'cat /opt/ca.crt' > ca.crt
vagrant ssh -c 'cat /opt/client.crt' > client.crt
vagrant ssh -c 'cat /opt/client.key' > client.key
vagrant ssh -c 'cat /opt/dh2048.pem' > dh2048.pem
```
- установить openvpn
```
apt-get install openvpn easy-rsa -y
```
- Запустить клиент
```
openvpn --config ./client.conf &
```

После старта проверяем доступ.

![Image_alt](https://github.com/kenttok/LP_Lesson34/blob/main/2.png)
