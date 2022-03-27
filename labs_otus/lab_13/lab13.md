# Лабораторная работа N6
# Внедрение маршрутизации между виртуальными локальными сетями

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети |Шлюз по умолчанию |
|------|--------|-------|-------|-----|
| R1   | G0/0/1.10 | 192.168.10.1 |255.255.255.0     | —       |
|      | G0/0/1.20 | 192.168.20.1 |255.255.255.0     | —       |
|      | G0/0/1.30 | 192.168.30.1 |255.255.255.0     | —       |
|      | G0/0/1.1000 |—   | —    | —       |
| S1   |VLAN 10  |192.168.10.11  |255.255.255.0     | 192.168.10.1      |
| S2   |VLAN 10  |192.168.10.12  |255.255.255.0     | 192.168.10.1      |
| PC-A |NIC     |192.168.20.3 |255.255.255.0     | 192.168.20.1 |
| PC-B |NIC     |192.168.30.3 |255.255.255.0     | 192.168.30.1 |

**Таблица VLAN**

|VLAN|Имя|IP адрес |
|------|--------|-------|
| 10   | G0/0/1.10 | S1: VLAN 10  |
| 10   | G0/0/1.10 | S2: VLAN 10  |
| 20     | Sales| S1: F0/6 |
| 30     | Operations | S2: F0/18 |
| 999     | Parking_Lot |С1: F0/2-4, F0/7-24, G0/1-2  | 
| 999     | Parking_Lot |С2: F0/2-17, F0/19-24, G0/1-2   | 
| 1000   |Собственная  |-  |


**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Создание сетей VLAN и назначение портов коммутатора*

*Часть 3. Настройка транка 802.1Q между коммутаторами.*

*Часть 4. Настройка маршрутизации между сетями VLAN*

*Часть 5. Проверка, что маршрутизация между VLAN работает*

**Шаг Настройте базовые параметры для маршрутизатора**

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

> R1(config)#exit

> R1#copy running-config startup-config

> R1#clock set 15:05:00 27 Mar 2022


  

**Шаг Настройте базовые параметры каждого коммутатора.**

**коммутатора S1**

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

> S1(config)#copy running-config startup-config

> S1(config)#exit

> S1#clock set 15:05:00 27 Mar 2022


**коммутатора S2**

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

> S2(config)#copy running-config startup-config

> S2(config)#exit

> S2#clock set 15:05:00 27 Mar 2022

**Настройте узлы ПК.**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/2.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/3.jpg "")

**Создайте сети VLAN на коммутаторах.**


**Назначьте сети VLAN соответствующим интерфейсам коммутатора.**

**Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.**

**Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.**

**Настройте маршрутизатор.**
