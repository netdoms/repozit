# Лабораторная работа N26
# Настройка NAT для IPv4

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес     |Маска подсети  |
|------|-----------|---------------|---------------|
|R1    |Gi0/0      |209.165.200.230|255.255.255.248|
|      |Gi0/1      |192.168.1.1    |255.255.255.0  |
|R2    |Gi0/0      |209.165.200.225|255.255.255.248|
|      |lo1        |209.165.200.1  |255.255.255.0  |
| S1   |VLAN 20    |192.168.1.11   |255.255.255.0  |
| S2   |VLAN 20    |192.168.1.12   |255.255.255.0  |
| PC-A |NIC        |192.168.1.2    |255.255.255.0  |
| PC-B |NIC        |192.168.1.3    |255.255.255.0  |


**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Настройка и проверка NAT для IPv4*

*Часть 3. Настройка и проверка PAT для IPv4*

*Часть 4. Настройка и проверка статического NAT для IPv4.*

=====================================================================================

**Часть 1. Создание сети и настройка основных параметров устройства**

**Настройка базовые параметры для маршрутизатора R1**

> Router>enable 

> Router#configure terminal

> Router(config)#no ip domain-lookup

> Router(config)#hostname R1

> R1(config)#line con 0

> R1(config-line)#logging synchronous

> R1(config-line)#password cisco

> R1(config-line)#login

> R1(config)#line vty 0 4

> R1(config-line)#password cisco

> R1(config-line)#login

> R1(config-line)#transport input all

> R1(config-line)#exit

> R1(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. #

> R1(config)#enable secret class

> R1(config)#service password-encryption

> R1(config)#exit

> R1#copy running-config startup-config



**Настройка базовые параметры для маршрутизатора R2**

> Router>enable 

> Router#configure terminal

> Router(config)#no ip domain-lookup

> Router(config)#hostname R2

> R2(config)#line con 0

> R2(config-line)#logging synchronous

> R2(config-line)#password cisco

> R2(config-line)#login

> R2(config)#line vty 0 4

> R2(config-line)#password cisco

> R2(config-line)#login

> R2(config-line)#transport input all

> R2(config-line)#exit

> R2(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. #

> R2(config)#enable secret class

> R2(config)#service password-encryption

> R2(config)#exit

> R2#copy running-config startup-config

> R2#clock set 15:05:00 01 june 2022

**Настройте базовые параметры каждого коммутатора.**

**коммутатора базовые параметры S1**

> Switch>enable 

> Switch#configure terminal

> Switch(config)#no ip domain-lookup

> Switch(config)#hostname S1

> S1(config)#line con 0

> S1(config-line)#logging synchronous

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config)#line vty 0 4

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config-line)#transport input all

> S1(config-line)#exit

> S1(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. # 

> S1(config)#enable secret class

> S1(config)#service password-encryption

> S1(config)#exit

> S1(config)#interface range Gi1/0-3

> S1(config-if-range)#switchport mode access

> S1(config-if-range)#shutdown

> S1#copy running-config startup-config

**коммутатора базовые параметры S2**

> Switch>enable 

> Switch#configure terminal

> Switch(config)#no ip domain-lookup

> Switch(config)#hostname S2

> S2(config)#line con 0

> S2(config-line)#logging synchronous

> S2(config-line)#password cisco

> S2(config-line)#login

> S2(config)#line vty 0 4

> S2(config-line)#password cisco

> S2(config-line)#login

> S2(config-line)#transport input all

> S2(config-line)#exit

> S2(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. # 

> S2(config)#enable secret class

> S2(config)#service password-encryption

> S2(config)#exit

> S2(config)#interface range Gi0/2, Gi1/0-3

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#shutdown

> S2#copy running-config startup-config


=======================================================================================

**R1**

> R1(config)#ip route 0.0.0.0 0.0.0.0 209.165.200.225

> R1(config)#interface gi0/1


> R1(config-if)#ip address 192.168.1.1 255.255.255.0

> R1(config-if)#no shutdown


> R1(config)#interface gi0/0


> R1(config-if)#ip address 209.165.200.230 255.255.255.248

> R1(config-if)#no shutdown

**R2**


> R2(config)#interface gi0/0

> R2(config-if)#ip address 209.165.200.225 255.255.255.248

> R2(config-if)#no shutdown

> R2(config)#interface loopback1


> R2(config-if)#ip address 209.165.200.1 255.255.255.224

**S1**

> S1(config)#interface vlan1


> S1(config-if)#ip address 192.168.1.11 255.255.255.0

> S1(config-if)no shutdown


> S1(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.1


**S2**

> S2(config)#interface vlan1


> S2(config-if)#ip address 192.168.1.12 255.255.255.0

> S2(config-if)no shutdown

> S2(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.1


**Часть 2. Настройка и проверка NAT для IPv4.**

**Шаг 1. Настройте NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228**

> R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255

> R1(config)#ip route 0.0.0.0 0.0.0.0 209.165.200.225

> R1(config)#ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248

> R1(config)#ip nat inside source list 1 pool PUBLIC_ACCESS

> R1(config)#interface gi0/1

> R1(config-if)#ip nat inside

> R1(config-if)#exit

> R1(config)#interface gi0/0

> R1(config-if)#ip nat outside

> R1(config-if)#

**Шаг 2. Проверьте и проверьте конфигурацию.** 

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/2.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/3.jpg "")

*Во что был транслирован внутренний локальный адрес PC-B?*

Адрес был транслирован в 1 из 3 свободных адресов внутреннего глобального пула 209.165.200.226

*Какой тип адреса NAT является переведенным адресом?*

динамическим адресом.


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/4.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/5.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/6.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/7.jpg "")


> R1#clear ip nat translation *

> R1#clear ip nat statistics

**Часть 3. Настройка и проверка PAT для IPv4**

> R1#conf t

> R1(config)#no ip nat inside source list 1 pool PUBLIC_ACCESS

*Во что был транслирован внутренний локальный адрес PC-B?*
 
Во внутрений глобальный адрес + порт

*Какой тип адреса NAT является переведенным адресом?*

Преобразование адресов портов

*Чем отличаются выходные данные команды show ip nat translations из упражнения NAT?*

В адресе присутствует порт

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/8.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/9.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/10.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/11.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/12.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/13.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/14.jpg "")

*Как маршрутизатор отслеживает, куда идут ответы?*

По  номеру портов.

> R1#clear ip nat translation *

> R1#clear ip nat statistics

**Шаг 4. На R1 удалите команды преобразования nat pool.**

> R1#conf t


> R1(config)#no ip nat inside source list 1 pool PUBLIC_ACCESS overload

> R1(config)#no ip nat pool PUBLIC_ACCESS

> R1(config)#ip nat inside source list 1 interface gi0/0 overload

> S1#ping 209.165.200.1 repeat 999

> S2#ping 209.165.200.1 repeat 999

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/15.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/16.jpg "")

**Часть 4. Настройка и проверка статического NAT для IPv4.**

> R1#clear ip nat translation *


> R1#clear ip nat statistics


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/17.jpg "")


> R2#ping 209.165.200.229 repeat 999


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_29/18.jpg "")