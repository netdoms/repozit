# Лабораторная работа N26
# Настройка и проверка расширенных списков контроля доступа

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|Шлюз      |
|------|-----------|-----------|-------------|----------|
|R1    |Gi0/1      |-          |-            |-         |
|      |Gi0/1.20   |10.20.0.1  |255.255.255.0|-         |
|      |Gi0/1.30   |10.30.0.1  |255.255.255.0|-         |
|      |Gi0/1.40   |10.40.0.1  |255.255.255.0|-         |
|      |Gi0/1.1000 |-          |-            |-         |
|      |Loopback1  |172.16.1.1 |255.255.255.0|-         |
| R2   |Gi0/1      |10.20.0.4  |255.255.255.0|-         |
| S1   |VLAN 20    |192.168.1.2|255.255.255.0|10.20.0.1 |
| S2   |VLAN 20    |192.168.1.2|255.255.255.0|10.20.0.1 |
| PC-A |NIC        |10.30.0.10 |255.255.255.0|10.30.0.1 |
| PC-B |NIC        |10.40.0.10 |255.255.255.0|10.40.0.1 |

**Таблица VLAM**

|VLan|Имя |Назначенный интерфейс|
|------|--------------|---------|
| 20   | Management   |S1: Gi0/1 |
| 30   | Operations   |S1: Gi0/3 |
| 40   |Sales        |S2: Gi0/2|
|999   |Parking_Lot |S1: Gi0/2, Gi1/0-3 |
|      |            |S2: Gi0/3, Gi1/0-3|
|1000  |Собственная  |-|

**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Настройка и проверка списков расширенного контроля доступа*



> Router>enable 

> Router#configure terminal

> Router(config)#no ip domain-lookup

> Router(config)#hostname R1

> R1(config)#ip domain name ccna-lab.com 

> R1(config)#crypto key generate rsa

> R1(config)#username SSHadmin privilege 1 password $cisco123! 

> R1(config)#line con 0

> R1(config-line)#logging synchronous

> R1(config-line)#password cisco

> R1(config-line)#login

> R1(config)#line vty 0 4


>  R1(config-line)#login local

> R1(config-line)#transport input ssh

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

> R2(config)#ip domain name ccna-lab.com 

> R2(config)#crypto key generate rsa

> R2(config)#username SSHadmin privilege 1 password $cisco123! 

> R2(config)#line con 0

> R2(config-line)#logging synchronous

> R2(config-line)#password cisco

> R2(config-line)#login

> R2(config)#line vty 0 4

> R2(config-line)#login local

> R2(config-line)#transport input ssh

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

> S1(config)#ip domain name ccna-lab.com 

> S1(config)#crypto key generate rsa

> S1(config)#username SSHadmin privilege 1 password $cisco123! 


> S1(config)#line con 0

> S1(config-line)#logging synchronous

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config)#line vty 0 4

>  S1(config-line)#login local

> S1(config-line)#transport input ssh

> S1(config-line)#exit

> S1(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. # 

> S1(config)#enable secret class

> S1(config)#service password-encryption

> S1(config)#exit

> S1#copy running-config startup-config

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/2.jpg "")

**коммутатора базовые параметры S2**

> Switch>enable 

> Switch#configure terminal

> Switch(config)#no ip domain-lookup

> Switch(config)#hostname S2

> S2(config)#ip domain name ccna-lab.com 

> S2(config)#crypto key generate rsa

> S2(config)#username SSHadmin privilege 1 password $cisco123! 

> S2(config)#line con 0

> S2(config-line)#logging synchronous

> S2(config-line)#password cisco

> S2(config-line)#login

> S2(config)#line vty 0 4

> S2(config-line)#login local

> S2(config-line)#transport input ssh

> S2(config-line)#exit

> S2(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. # 

> S2(config)#enable secret class

> S2(config)#service password-encryption

> S2(config)#exit

> S2#copy running-config startup-config

#####################################################################################
#####################################################################################

**НАСТРОЙКА МАРШРУТИЗАТОРА R1**

**R1  Gi0/0/1**


> R1(config)#interface Gi0/1.20

> R1(config-subif)#encapsulation dot1Q 20

> R1(config-subif)#ip address 10.20.0.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-if)#exit



> R1(config)#interface Gi0/1.30

> R1(config-subif)#encapsulation dot1Q 30

> R1(config-subif)#ip address 10.30.0.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-subif)#exit

> R1(config)#interface Gi0/1.40

> R1(config-subif)#encapsulation dot1Q 40

> R1(config-subif)#ip address 10.40.0.1 255.255.255.0

> R1(config-subif)#no shutdown

> R1(config-subif)#exit


> R1(config)#interface Gi0/1.1000

> R1(config-subif)#no shutdown

> R1(config-subif)#encapsulation dot1Q 1000 native

> R1(config-subif)#exit

> R1(config)#interface Gi0/1

>R1(config-if)#no shutdown

    *Jun 14 18:00:17.229: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun 14 18:00:18.229: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

> R1(config)#interface Loopback1

    *Jun 14 18:01:01.286: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up


> R1(config-if)#ip address 172.16.1.1 255.255.255.0

> R1(config-if)#end

> R1#wr

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/3.jpg "")

#####################################################################################
#####################################################################################

**НАСТРОЙКА МАРШРУТИЗАТОРА R2**

**R2  Gi0/0/1#**

> R2(config)#interface Gi0/1

> R2(config-if)#ip address 10.20.0.4 255.255.255.0

> R2(config-if)#no shutdown


    *Jun 14 18:08:05.215: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun 14 18:08:06.215: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

> R2(config-if)#end

**НАСТРОЙКА МАРШРУТов по умолчанию**

> R2(config)#ip route 0.0.0.0 0.0.0.0 10.20.0.1

##########################################################################################
#####################################################################



**АСТРОЙКА КОММУТАТОРА S2**

**S2 Vlan**

> S2(config)#interface vlan 20


> S2(config-if)#ip address 10.20.0.3 255.255.255.0

> S2(config-if)#ip default-gateway 10.20.0.1

> S2(config-if)#no shutdown

> S2(config-if)#exit

> S2(config)#interface vlan 1000

> S2(config-if)#no shutdown

    *Jun 14 19:04:20.216: %LINK-3-UPDOWN: Interface Vlan1000, changed state to down


**S2 port Gi0/0**

> S2(config)#interface Gi0/0

> S2(config)#switchport trunk encapsulation dot1q

> S2(config-if)#switchport mode trunk

> S2(config-if)#Switchport trunk native vlan 1000

> S2(config-if)#switchport trunk allowed vlan 20,30,40,1000

> S2(config-if)#exit


**S2 port Gi0/2**

> S2(config)#interface vlan 40

> S2(config-if)#no shutdown

> S2(config-if)#exit

> S2(config)#interface Gi0/2

> S2(config-if)#switchport mode access

> S2(config-if)#switchport access vlan 40

> S2(config-if)#exit

**S2  port Gi0/1**

> S2(config)#interface Gi0/1

> S2(config)#switchport trunk encapsulation dot1q

> S2(config-if)#switchport mode trunk

> S2(config-if)#Switchport trunk native vlan 1000

> S2(config-if)#switchport trunk allowed vlan 20,1000

> S2(config-if)#exit

##########################################################################

**S2    ОТКЛЮЧЕНИЕ НЕИСПОЛЬЗОВАННЫХ ПОРТОВ**

> S2(config)#interface vlan 999

> S2(config-if)#no shutdown


> S2(config)#interface range Gi0/3, Gi1/0-3

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#switchport access vlan 999

> S2(config-if-range)#shutdown


    *Jun 14 19:14:22.561: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun 14 19:14:22.603: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun 14 19:14:22.642: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun 14 19:14:22.683: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun 14 19:14:22.723: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun 14 19:14:23.562: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S2(config-if-range)#
    *Jun 14 19:14:23.603: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun 14 19:14:23.643: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun 14 19:14:23.683: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun 14 19:14:23.723: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down


#####################################################################################
#####################################################################################

**НАСТРОЙКА КОММУТАТОРА S1**

**Vlan**

> S1(config)#interface vlan 20

> S1(config-if)#ip address 10.20.0.2 255.255.255.0

> S1(config-if)#ip default-gateway 10.20.0.1

> S1(config-if)#no shutdown

> S1(config)#vlan 30

> S1(config-vlan)#end


> S1(config)# vlan 40

> S1(config-vlan)#end



> S1(config)# vlan 999

> S1(config-vlan)#end


> S1(config)# vlan 1000

> S1(config-vlan)end

**S1 shutdown port Gi0/0**

> S1(config)#interface Gi0/0

> S1(config-if)#switchport trunk encapsulation dot1q


> S1(config-if)#switchport mode trunk


> S1(config-if)#Switchport trunk native vlan 1000

> S1(config-if)#switchport trunk allowed vlan 20,30,40,1000


**S1 shutdown port Gi0/1**

> S1(config)#interface Gi0/1

> S1(config-if)#switchport trunk encapsulation dot1q

> S1(config-if)#switchport mode trunk

> S1(config-if)#Switchport trunk native vlan 1000

> S1(config-if)#switchport trunk allowed vlan 20,30,40,1000

> S1(config-if)#exit


**S1 shutdown port Gi0/3**

> S1(config)#interface Gi0/3

> S1(config-if)#switchport mode access

> S1(config-if)#switchport access vlan 30

> S1(config-if)#exit

**S1 ОТКЛЮЧЕНИЕ НЕ ИСПОЛЬЗУЕМЫХ ПОРТОВ**

> S1(config)#interface range Gi0/2, Gi1/0-3

> S1(config-if-range)#switchport mode access

> S1(config-if-range)#switchport access vlan 999

> S1(config-if-range)#shutdown

    *Jun 14 18:57:54.851: %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    *Jun 14 18:57:54.893: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun 14 18:57:54.935: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun 14 18:57:54.975: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun 14 18:57:55.019: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun 14 18:57:55.852: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
    S1(config-if-range)#
    *Jun 14 18:57:55.893: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun 14 18:57:55.935: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun 14 18:57:55.975: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun 14 18:57:56.019: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down


> S1(config-if-range)#exit


##########################################################################

**Включите защищенные веб-службы с проверкой подлинности на R1**

> R1(config)#ip http secure-server

    % Generating 1024 bit RSA keys, keys will be non-exportable...
    [OK] (elapsed time was 0 seconds)
    Failed to generate persistent self-signed certificate.
        Secure server will use temporary self-signed certificate.


> R1(config)#ip http authentication local

**Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/4.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/5.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/6.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/7.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_26/8.jpg "")


