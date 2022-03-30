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

> S1(config)#copy running-config startup-config

> S1(config)#exit

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

> S2(config)#exit

> S2#copy running-config startup-config

> S2#clock set 15:05:00 27 Mar 2022

**Настройте узлы ПК.**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/2.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/3.jpg "")

**НАСТРОЙКА S1**

**S1 vlan1**

> S1(config)#interface vlan10

> S1(config-if)#ip address 192.168.1.11 255.255.255.0

> S1(config-if)#ip default-gateway 192.168.10.1

> S1(config-if)#exit

> S1(config)#interface vlan10

> S1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

**VLAN  S1**

> S1(config)#vlan 10

> S1(config-vlan)#name cisco

> S1(config)#vlan 20

> S1(config-vlan)#name Sales

> S1(config)#vlan 30

> S1(config-vlan)#name Operations

> S1(config)#vlan 1000

> S1(config-vlan)#name native

> S1(config-vlan)#vlan 999

> S1(config)#interface range F0/2-4, F0/7-24, G0/1-2

> S1(config-if-range)#switchport mode access

> S1(config-if-range)#switchport access vlan 999

> S1(config-if-range)#shutdown

    %LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

    %LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

    %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down

**S1 интерфейсы**    

> S1(config)#interface f0/6

> S1(config-if)#switchport mode  access

> S1(config-if)#switchport access vlan 20

**Транк S1 f0/5**
> S1(config-if)#interface f0/5

> S1(config-if)#switchport mode trunk

> S1(config-if)#Switchport trunk native vlan 1000

> S1(config-if)#Switchport trunk allowed vlan 10,20,30,1000

**Транк S1 f0/1**

> S1(config)#interface f0/1

> S1(config-if)#switchport mode trunk

    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

> S1(config-if)#switchport mode trunk

> S1(config-if)#Switchport trunk native vlan 1000

    %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (1000), with S2 FastEthernet0/1 (1).

> S1(config-if)#Switchport trunk allowed vlan 10,20,30,1000

**НАСТРОЙКА S2** 

**S2**   

> S2(config)#interface vlan10

> S2(config-if)#ip address 192.168.1.12 255.255.255.0

> S2(config-if)#ip default-gateway 192.168.10.1

> S2(config)#interface vlan10

> S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

> S2(config)#interface f0/18

> S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface FastEthernet0/18, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/18, changed state to up

**VLAN  S2**
> S2(config)#vlan 10

> S2(config-vlan)#name cisco

> S2(config)#vlan 20

> S2(config-vlan)#name Sales

> S2(config)#vlan 30

> S2(config-vlan)#name Operations

> S2(config-if)#switchport mode  access

> S2(config-if)#switchport access vlan 30

> S2(config-if)#exit

> S2(config)#vlan 1000

> S2(config-vlan)#name native

> S2(config-vlan)#vlan 999

> S2(config)#interface f0/2-17, f0/19-24, g0/1-2

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#switchport access vlan 999

> S2(config-if-range)#shutdown

**Транк S2 f0/1**

> S2(config)#interface f0/1

> S2(config-if)#switchport mode trunk

> S2(config-if)#Switchport trunk native vlan 1000

> S2(config-if)#Switchport trunk allowed vlan 10,20,30,1000


**НАСТРОЙКА МАРШРУТИЗАТОРА S1.**

> R1(config)#interface G0/0/1.10

> R1(config-subif)#encapsulation dot1Q 10

> R1(config-subif)#ip address 192.168.10.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-subif)#exit

> R1(config)#interface G0/0/1.20

> R1(config-subif)#encapsulation dot1Q 20

> R1(config-subif)#ip address 192.168.20.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-subif)#exit

> R1(config)#interface G0/0/1.30

> R1(config-subif)#encapsulation dot1Q 30

> R1(config-subif)#ip address 192.168.30.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-subif)#exit

> R1(config)#interface G0/0/1.1000

> R1(config-subif)#no shutdown

> R1(config-subif)#encapsulation dot1Q 1000 native

> R1(config-subif)#exit

> R1(config)#interface G0/0/1

> R1(config-if)#no shutdown 

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.10, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.10, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.20, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.20, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.30, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.30, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

**Проверьте, работает ли маршрутизация между VLAN**

*Отправьте эхо-запрос с PC-A на шлюз по умолчанию.*

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/7.jpg "")


Отправьте эхо-запрос с PC-A на PC-B.

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/8.jpg "")

Отправьте команду ping с компьютера PC-A на коммутатор S2.

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/9.jpg "")

Виртуальный интерфейс свитчей пингуется только на самом свитче.

В окне командной строки на PC-B выполните команду tracert на адрес PC-A.

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_13/6.jpg "")

Какие промежуточные IP-адреса отображаются в результатах?

    192.168.30.1   192.168.20.3