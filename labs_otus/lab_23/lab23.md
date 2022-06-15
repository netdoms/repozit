# Лабораторная работа N23
# Настройка протокола OSPFv2 

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|
|------|-----------|-----------|-------------|
| R1   | G0/0/1    | 10.53.0.1 |255.255.255.0|
|      |Loopback 1 |172.16.1.1 |255.255.255.0|
| R2   | G0/0/1    |10.53.0.2  |255.255.255.0|
|      | Loopback1 |192.168.1.1|255.255.255.0|


**Задачи**

*Создание сети и настройка основных параметров устройства*

*Настройка и проверка базовой работы протокола  OSPFv2 для одной области*

*Оптимизация и проверка конфигурации OSPFv2 для одной области*

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

**Часть 1. Создание сети и настройка основных параметров устройства**

**R1**

> R1(config)# interface G0/0/1

> R1(config-if)#ip address 10.53.0.1 255.255.255.0

> R1(config-if)#no shutdown

> R1(config-if)#

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

> R1(config-if)#exit

> R1(config)#interface Loopback1

> R1(config-if)#

    %LINK-5-CHANGED: Interface Loopback1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

> R1(config-if)#ip address 172.16.1.1  255.255.255.0

> R1(config-if)#exit

> R1(config)#end

> R2(config)# interface G0/0/1

> R2(config-if)#ip address 10.53.0.2 255.255.255.0

> R2(config-if)#no shutdown

> R2(config-if)#

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

> R2(config-if)#exit

> R2(config)#interface Loopback1

> R2(config-if)#

    %LINK-5-CHANGED: Interface Loopback1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

> R2(config-if)#ip address 192.168.1.1  255.255.255.0

> R2(config-if)#exit

> R2(config)#end


**Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области**

> R1(config)#router ospf 56

> R1(config-router)# router-id 1.1.1.1

> R1(config-router)#network 10.53.0.0 0.0.0.255 area 0

    04:08:56: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done


> R2(config)#router ospf 56

> R2(config-router)#router-id 2.2.2.2

> R2(config-router)#network 10.53.0.0 0.0.0.255 area 0

    04:08:56: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

> R2(config)#interface Loopback1

> R2(config-if)#ip ospf 56 area 0

> R2(config-if)#end


> R2#show ip ospf neighbor 

    Neighbor ID     Pri   State           Dead Time   Address         Interface
    1.1.1.1           1   FULL/DR         00:00:34    10.53.0.1       GigabitEthernet0/0/1

Роутер R2 BDR.

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/2.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/3.jpg "")

**Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области**

> R1(config)#interface G0/0/1

> R1(config-if)#ip ospf priority 50

> R1(config-if)#ip ospf dead-interval 120

> R1(config-if)#ip ospf hello-interval 30

    07:41:18: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Dead timer expired

    07:41:18: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached




> R2(config)#interface G0/0/1

> R2(config-if)#ip ospf priority 50

> R2(config-if)#ip ospf hello-interval 30

> R2(config-if)#ip ospf dead-interval 120

> R1(config)#ip route 0.0.0.0 0.0.0.0 loopback1

    %Default route without gateway, if not a point-to-point interface, may impact performance

> R1(config)#router ospf 56

> R1(config-router)#default-information originate

> R2(config)#interface loopback1

> R2(config-if)#ip ospf network point-to-point

> R2(config-if)#end

> R2(config)#router ospf 56

> R2(config-router)#passive-interface loopback1

> R1(config)#router ospf 56

> R1(config-router)#auto-cost reference-bandwidth 10000

    % OSPF: Reference bandwidth is changed.
            Please ensure reference bandwidth is consistent across all routers.

 R2(config-router)#auto-cost reference-bandwidth 10000

    % OSPF: Reference bandwidth is changed.
            Please ensure reference bandwidth is consistent across all routers.            


> R1#clear ip ospf process

> Reset ALL OSPF processes? [no]: y

    00:35:26: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

    00:35:26: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

    00:35:30: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done


> R2#clear ip ospf process

> Reset ALL OSPF processes? [no]: y


    00:37:04: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

    00:37:04: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

    00:37:30: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/4.jpg "") 

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_23/5.jpg "")   


Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24?

Метрика не меняется. Пассивный интерфейс