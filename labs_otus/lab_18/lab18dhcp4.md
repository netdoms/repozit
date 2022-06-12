# Лабораторная работа N8
# Протоколы DHCPv4

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/1d.jpg "")

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

> S2#clock set 15:05:00 01 june 2022

> S2#copy running-config startup-config

# НАСТРОЙКА МАРШРУТИЗАТОРА R1#

#  R1  Gi0/1#



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

R1(config)#interface Gi0/0/1

R1(config-if)#no shutdown

R1(config-if)#

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.100, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.100, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.200, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.200, changed state to up

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up














# R1  Gi0/0#

> R1(config)#interface Gi0/0/0

> R1(config-if)#ip address 10.0.0.1 255.255.255.252

> R1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

> R1(config-if)#exit








# НАСТРОЙКА МАРШРУТИЗАТОРА R2#

# R2  Gi0/0#

> R2(config)#interface Gi0/0/0

> R2(config-if)#ip address 10.0.0.2 255.255.255.252

> R2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

> R2(config-if)#exit

#  R2  Gi0/1#

> R2(config)#interface Gi0/0/1

> R2(config-if)#ip address 192.168.1.97 255.255.255.240

> R2(config-if)#no shutdown

%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

> R2(config-if)#end

> # НАСТРОЙКА МАРШРУТов по умолчанию#

> R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1

> R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2

    R2#ping 10.0.0.1
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 10.0.0.1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 4/8/12 ms


# НАСТРОЙКА S2#



# fa0/18 S-PC#

S2(config-if)#interface fa0/18

S2(config-if)#switchport mode access

S2(config-if)#Switchport trunk allowed vlan 1

S2(config-if)#no shutdown

S2(config-if)#exit

S2(config)#vlan 1
S2(config-vlan)#interface fa0/18
S2(config-if)# no shutdown

S2(config-if)#
%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/18, changed state to up




# S2 Vlan #

S2(config)#interface vlan1

    *Jun 10 09:12:14.500: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down

S2(config-if)#ip address 192.168.1.98 255.255.255.240

S2(config-if)#ip default-gateway 192.168.1.97

S2(config-if)#no shutdown

S2(config-if)#exit

S2(config)#interface vlan1

S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


# S2 shutdown port f0/5#

S1(config)#interface f0/5
S1(config-if)#switchport mode trunk
S1(config-if)#Switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 1,1000













# S2 shutdown port#

S2(config)#interface range Gi0/0-1, Gi1/0-3

S2(config-if-range)#switchport trunk encapsulation dot1q

S2(config-if-range)#switchport mode access

S2(config-if)#shutdown

    *Jun 10 09:11:23.391: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
    *Jun 10 09:11:23.474: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down
    *Jun 10 09:11:23.543: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun 10 09:11:23.658: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun 10 09:11:23.732: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun 10 09:11:23.799: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun 10 09:11:24.393: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to down
    S2(config-if-range)#
    *Jun 10 09:11:24.474: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
    *Jun 10 09:11:24.542: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun 10 09:11:24.686: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun 10 09:11:24.735: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun 10 09:11:24.804: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down



 


# НАСТРОЙКА S1#

#  Vlan #

S1(config)#interface vlan 200

S1(config-if)#ip address 192.168.1.66 255.255.255.224

S1(config-if)#ip default-gateway 192.168.1.65

S1(config-if)#no shutdown

S1(config)#vlan 200

S1(config-vlan)#

    %LINK-5-CHANGED: Interface Vlan200, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, changed state to up




S1(config)# vlan 999

S1(config-vlan)#name ParkingLot


S1(config)# vlan 100

S1(config-vlan)#name customers

S1(config)# vlan 200

S1(config-vlan)#name Management

# Gi0/2 S-comp Gi0/3#

S1(config)#interface Gi0/3

S1(config-if)#switchport mode access

S1(config-if)#Switchport access vlan 200

S1(config-vlan)#exit

S1(config-if)#no shutdown

S1(config)#exit

S1(config-if)#shutdown

S1(config)#interface vlan 999

S2(config-vlan)#name ParkingLot



*Почему интерфейс F0/5 указан в VLAN 1?*







*Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?*

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/3.jpg "")

# Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. #


R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5

R1(config)#ip dhcp pool A-1

R1(config)#network 192.168.1.0 255.255.255.192

R1(config)#dns-server 192.168.1.1

R1(config)#default-router 192.168.1.1

R1(config)#domain-name CCNA-lab.com

R1(config)#end


R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.100



R1(config)#ip dhcp pool C-1

R1(config)#domain-name R2_Client_LAN 

R1(config)#network 192.168.1.0 255.255.255.240

R1(config)#default-router 192.168.1.97

R1(config)#dns-server 192.168.1.97
R1(config)#end

