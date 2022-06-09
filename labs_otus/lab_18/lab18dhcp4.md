# Лабораторная работа N8
# Протоколы DHCPv4

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|Шлюз|
|------|----------|------------|-------------|-------|
| R1   | Gi0/0    |10.0.0.1    |255.255.255.252|
|      | Gi0/1    |-           |-              |-            |
|      |Gi0/1.100 |192.168.1.1 |26             |-            |
|      |Gi0/1.200 |192.168.1.65|27             |-            |
|      |Gi0/1.1000|-           |-              |-            |
| R2   |Gi0/0     |10.0.0.2    |255.255.255.252|-            |
|      |Gi0/1     |192.168.1.97|28             |-            |
| S1   |VLAN 200  |192.168.1.66|27             |192.168.1.65 |
| S2   |VLAN 1    |192.168.1.2 |26             |192.168.1.1  |
| PC-A |NIC       |DHCP       |DHCP            |DHCP         |
| PC-B |NIC       |DHCP       |DHCP            |DHCP         |

**Таблица VLAM**

|VLan|Имя |Назначенный интерфейс|
|------|--------------|---------|
| 1    | нет       |S2 Gi1/3    |
|100   |Клиенты    |S1:Gi1/2    |
|200   |Управление |S1:VLAN 200 |
|999   |Parking_Lot |S1: Gi1/0-3 Gi0/1, Gi0/3 |
|1000  |Собственная  |-|

**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Настройка и проверка двух серверов DHCPv4 на R1*

*Часть 3. Настройка и проверка DHCP-ретрансляции на R2*

*Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1*

*Часть 5. Настройка и проверка DHCPv6 Relay на R2*

**Шаг Настройте базовые параметры для маршрутизатора R1**

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



**Шаг Настройте базовые параметры для маршрутизатора R2**

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

**Шаг Настройте базовые параметры каждого коммутатора.**

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

> S1(config)#copy running-config startup-config

> S1#clock set 15:05:00 27 Mar 2022

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

> S2#copy running-config startup-config

