# Лабораторная работа N23
# Протоколы DHCPv4

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|Шлюз|
|------|----------|------------|-------------|-------|
| R1   | Gi0/0    |10.0.0.1    |255.255.255.252|
|      | Gi0/1    |-           |-              |-            |
|      |Gi0/1.100 |192.168.1.1 |255.255.255.192|-            |
|      |Gi0/1.200 |192.168.1.65|255.255.255.224|-            |
|      |Gi0/1.1000|-           |-              |-            |
| R2   |Gi0/0     |10.0.0.2    |255.255.255.252|-            |
|      |Gi0/1     |192.168.1.97|255.255.255.240|-            |
| S1   |VLAN 200  |192.168.1.66|255.255.255.224|192.168.1.65 |
| S2   |VLAN 1    |192.168.1.2 |255.255.255.240|192.168.1.97 |
| PC-A |NIC       |DHCP       |DHCP            |DHCP         |
| PC-B |NIC       |DHCP       |DHCP            |DHCP         |

**Таблица VLAM**

|VLan|Имя |Назначенный интерфейс|
|------|--------------|---------|
| 1    | нет       |S2: F0/18    |
|100   |Клиенты    |S1: F0/6     |
|200   |Управление |S1:VLAN 200 |
|999   |Parking_Lot |S1: F0/1-4, F0/7-24, G0/1-2 |
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

> S1#copy running-config startup-config

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

> S2#clock set 15:05:00 01 june 2022

> S2#copy running-config startup-config

**НАСТРОЙКА МАРШРУТИЗАТОРА R1**

**R1  Gi0/0/1**

> R1(config)#interface Gi0/0/1.100

> R1(config-subif)#encapsulation dot1Q 100

> R1(config-subif)#ip address 192.168.1.1 255.255.255.192

> R1(config-subif)#description customers

> R1(config-subif)#no shutdown

> R1(config-if)#exit



> R1(config)#interface Gi0/0/1.200

> R1(config-subif)#encapsulation dot1Q 200

> R1(config-subif)#ip address 192.168.1.65 255.255.255.224

> R1(config-subif)#description customers

> R1(config-subif)#no shutdown

> R1(config-subif)#exit


> R1(config)#interface Gi0/0/1.1000

> R1(config-subif)#no shutdown

> R1(config-subif)#encapsulation dot1Q 1000 native

> R1(config-subif)#description native

> R1(config-subif)#exit

> R1(config)#interface Gi0/0/1

>R1(config-if)#no shutdown

> R1(config-if)#


    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.100, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.100, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.200, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.200, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up



**R1  Gi0/0/0**

> R1(config)#interface Gi0/0/0

> R1(config-if)#ip address 10.0.0.1 255.255.255.252

> R1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

> R1(config-if)#exit

#####################################################################################

**НАСТРОЙКА МАРШРУТИЗАТОРА R2**

**R2  Gi0/0/0**

> R2(config)#interface Gi0/0/0

> R2(config-if)#ip address 10.0.0.2 255.255.255.252

> R2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

> R2(config-if)#exit

**R2  Gi0/0/1#**

> R2(config)#interface Gi0/0/1

> R2(config-if)#ip address 192.168.1.97 255.255.255.240

> R2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

> R2(config-if)#end

##########################################################################


**НАСТРОЙКА МАРШРУТов по умолчанию**

> R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1

> R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2

    R2#ping 10.0.0.1
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 10.0.0.1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 4/8/12 ms


##########################################################################    


**АСТРОЙКА КОММУТАТОРА S2**

**S2 Vlan**

> S2(config)#interface vlan1

    *Jun 10 09:12:14.500: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down

> S2(config-if)#ip address 192.168.1.98 255.255.255.240

> S2(config-if)#ip default-gateway 192.168.1.97

> S2(config-if)#no shutdown

> S2(config-if)#exit

> S2(config)#interface vlan1

> S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


##########################################################################

**S2    ОТКЛЮЧЕНИЕ НЕИСПОЛЬЗОВАННЫХ ПОРТОВ**

> S2(config)#S2(config)#interface range f0/1-4, f0/6-17, f0/19-24, g0/1-2

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#shutdown


**НАСТРОЙКА КОММУТАТОРА S1**

**Vlan**

> S1(config)#interface vlan 200

> S1(config-if)#ip address 192.168.1.66 255.255.255.224

> S1(config-if)#ip default-gateway 192.168.1.65

> S1(config-if)#no shutdown

> S1(config)#vlan 200

> S1(config-vlan)#

    %LINK-5-CHANGED: Interface Vlan200, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, changed state to up


> S1(config)# vlan 999

> S1(config-vlan)#name ParkingLot


> S1(config)# vlan 100

> S1(config-vlan)#name customers

> S1(config)# vlan 200

> S1(config-vlan)#name Management

**S1 shutdown port f0/5**

> S1(config)#interface f0/5

> S1(config-if)#switchport mode trunk

> S1(config-if)#Switchport trunk native vlan 1000

> S1(config-if)#switchport trunk allowed vlan 1,1000

**S1 ОТКЛЮЧЕНИЕ НЕ ИСПОЛЬЗУЕМЫХ ПОРТОВ**

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


*Почему интерфейс F0/5 указан в VLAN 1?*

Порт в состоянии trunk

*Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?*

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/2.jpg "")


##########################################################################

**Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей.**


> R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5

> R1(config)#ip dhcp pool A-1

> R1(config)#network 192.168.1.0 255.255.255.192

> R1(config)#dns-server 192.168.1.1

> R1(config)#default-router 192.168.1.1

> R1(config)#domain-name CCNA-lab.com

> R1(config)#lease 2 12 30

*lease This command was introduced. Release 12.3(8)T*

> R1(config)#end



> R1(config)#ip dhcp pool C-1

> R1(config)#domain-name R2_Client_LAN 

> R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.100

> R1(config)#network 192.168.1.96 255.255.255.240

> R1(config)#default-router 192.168.1.97

> R1(config)#dns-server 192.168.1.97

> R1(config)#lease 2 12 30

*lease This command was introduced. Release 12.3(8)T*

R1(config)#end

**Проверка конфигурации сервера DHCPv4**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/3.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/4.jpg "")


**Попытка получить IP-адрес от DHCP на PC-A**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/5.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/6.jpg "")

**Настройка и проверка DHCP-ретрансляции на R2** 


> R2(config)#interface gi0/0/1

> R2(config-if)#ip helper-address 10.0.0.1

> R2(config-if)#end

> R2#copy running-config startup-config

**Попытка получить IP-адрес от DHCP на PC-B**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/7.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/4.jpg "")
