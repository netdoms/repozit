# Лабораторная работа N20
# Принципы обеспечения безопасности сети

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети |
|------|--------|---------------|-------------|
| R1   | Gi0/1  | 192.168.10.1  |255.255.255.0|
|      | Loopback 0  |10.10.1.1 |255.255.255.0|
| S1   | VLAN 10|192.168.10.201 |255.255.255.0|
| S2   | VLAN 10|192.168.10.202 |255.255.255.0|
|      |VLAN 10 |192.168.10.11  |255.255.255.0|
| PC-A |NIC     |DHCP           |255.255.255.0|
| PC-B |NIC     |DHCP           |255.255.255.0|



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

> R1(config)#enable secret class

> R1(config)#service password-encryption

> R1(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.9

> R1(config)#ip dhcp excluded-address 192.168.10.201 192.168.10.202

> R1(config)#ip dhcp pool Students

> R1(dhcp-config)#network 192.168.10.0 255.255.255.0

> R1(dhcp-config)#default-router 192.168.10.1

> R1(dhcp-config)#domain-name CCNA2.Lab-11.6.1

> R1(dhcp-config)#interface Loopback0

> R1(config-if)#

    Interface Loopback0, changed state to up

>R1(config-if)#ip address 10.10.1.1 255.255.255.0

>R1(config-if)#interface Gig0/1

>R1(config-if)#description Link to S1

>R1(config-if)#ip dhcp relay information trusted

>R1(config-if)#ip address 192.168.10.1 255.255.255.0

>R1(config-if)#no shutdown

>R1(config-if)#

    *Jun  7 11:26:21.121: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun  7 11:26:22.128: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

> R1#copy running-config startup-config



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

> S1(config)#interface range Gi0/3, Gi1/0-3

> S1(config-if-range)#switchport mode access

> S1(config-if-range)#shutdown

    *Jun  7 10:42:06.605: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  7 10:42:06.639: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  7 10:42:06.689: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun  7 10:42:06.754: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  7 10:42:06.831: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  7 10:42:07.604: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S1(config-if-range)#
    *Jun  7 10:42:07.644: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun  7 10:42:07.691: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun  7 10:42:07.754: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun  7 10:42:07.833: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down

> S1(config-if-range)#exit

> S1(config)#exit

> S1#copy running-config startup-config

> S1(config)#exit

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

> S2(config)#interface range Gi0/3, Gi1/0-3

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#shutdown

    *Jun  7 10:59:24.952: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  7 10:59:25.018: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  7 10:59:25.093: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun  7 10:59:25.179: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  7 10:59:25.227: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  7 10:59:25.952: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S2(config-if-range)#
    *Jun  7 10:59:26.019: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun  7 10:59:26.095: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun  7 10:59:26.530: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun  7 10:59:26.534: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down

> S2(config-if-range)#exit

> S2(config)#exit

> S2#copy running-config startup-config



**НАСТРОЙКА S1**

**S1 vlan1**

> S1(config)#interface vlan10

> S1(config-if)#ip address 192.168.10.11 255.255.255.0

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
sho
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

*Что произойдет, если G0/0/1 н
а R1 будет отключен?*

Ничего

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

> S2(config-if)#ip address 192.168.10.12 255.255.255.0

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

