# Лабораторная работа N32
# Настройка протоколов CDP, LLDP и NTP

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|Шлюз      |
|------|-----------|-----------|-------------|----------|
|R1    |Loopback1  |172.16.1.1 |255.255.255.0|-         |
|      |Gi0/1      |10.22.0.1  |255.255.255.0|-         |
| S1   |VLAN 1     |10.22.0.2  |255.255.255.0|10.20.0.1 |
| S2   |VLAN 2     |10.22.0.3  |255.255.255.0|10.20.0.1 |

**Таблица VLAM**

**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP*

*Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP*

*Часть 4. Настройка и проверка NTP*


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

> R1(config)#interface Gi0/1

R1(config-if)#ip address 10.22.0.1 255.255.255.0   

> R1(config-if)#no shutdown


R1(config)#interface Loopback1


> R1(config-if)#ip address 172.16.1.1 255.255.255.0    

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

> S1(config)#exit

> S1#copy running-config startup-config



**коммутатора базовые параметры S2**

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

#####################################################################################
#####################################################################################

**Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/2.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/3.jpg "")

*Сколько интерфейсов участвует в объявлениях CDP?

GigabitEthernet0/1-3

 Какие из них активны?*

    GigabitEthernet0/1 is up, line protocol is up
    Encapsulation ARPA
    Sending CDP packets every 60 seconds
    Holdtime is 180 seconds


*Какая версия IOS используется на  S1?*


    Version :
    Cisco IOS Software, vios_l2 Software (vios_l2-ADVENTERPRISEK9-M), Version 15.2(4      .0.55)E, TEST ENGINEERING ESTG_WEEKLY BUILD, synced to  END_OF_FLO_ISP
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2015 by Cisco Systems, Inc.
    Compiled Tue 28-Jul-15 18:52 by sasyamal


##########################################################################################
#####################################################################



**НАСТРОЙКА КОММУТАТОРА S1**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/4.jpg "")


> S1(config)#interface vlan 1

> S1(config-if)#ip address 10.22.0.2 255.255.255.0

> S1(config-if)#ip default-gateway 10.22.0.1

> S1(config-if)#no sh

    S1(config-if)#
    *Jun 19 11:00:03.693: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
    *Jun 19 11:00:04.693: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up




#####################################################################################
#####################################################################################

**НАСТРОЙКА КОММУТАТОРА S2**

> S2(config)#interface vlan 1

> S2(config-if)#ip address 10.22.0.3 255.255.255.0

> S2(config-if)#ip default-gateway 10.22.0.1

> S2(config-if)#no sh


    *Jun 19 11:04:31.909: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
    *Jun 19 11:04:32.909: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/5.jpg "")


##########################################################################

> S1(config)#no cdp run

> S2(config)#no cdp run

> R1(config)#no cdp run

**Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP**

> S1(config)#lldp run

> S2(config)#lldp run

> R1(config)#lldp run

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/7.jpg "")

*Что такое chassis ID  для коммутатора S2*

some Cisco devices have default chassis ID values of their serial numbers

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/8.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/9.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/10.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/11.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/12.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/13.jpg "")

**Часть 4. Настройка NTP**

> R1#show clock

    *13:13:29.761 UTC Sun Jun 19 2022

> R1#clock set 16:17:00 01 june 2022

> R1#ntp server 10.22.0.1

> R1#show ntp status

    Clock is unsynchronized, stratum 16, no reference clock
    nominal freq is 1000.0003 Hz, actual freq is 1000.0003 Hz, precision is 2**16
    ntp uptime is 1600 (1/100 of seconds), resolution is 1000
    reference time is 00000000.00000000 (00:00:00.000 UTC Mon Jan 1 1900)
    clock offset is 0.0000 msec, root delay is 0.00 msec
    root dispersion is 0.24 msec, peer dispersion is 0.00 msec
    loopfilter state is 'NSET' (Never set), drift is 0.000000000 s/s
    system poll interval is 8, never updated.


> R1(config)#ntp master 4

    R1#show ntp status
    Jun 19 16:41:57.091: %SYS-5-CONFIG_I: Configured from console by console
    Clock is unsynchronized, stratum 4, reference is 127.127.1.1
    nominal freq is 1000.0003 Hz, actual freq is 1000.0003 Hz, precision is 2**16
    ntp uptime is 23200 (1/100 of seconds), resolution is 1000
    reference time is E659D04E.CE2105E8 (16:41:50.805 UTC Sun Jun 19 2022)
    clock offset is 0.0000 msec, root delay is 0.00 msec
    root dispersion is 937.81 msec, peer dispersion is 937.67 msec
    loopfilter state is 'FREQ' (Drift being measured), drift is 0.000000000 s/s
    system poll interval is 16, last update was 9 sec ago.

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/15.jpg "")

> S1(config)#ntp server 10.22.0.1 source Gi0/1

> S1#clock update-calendar

> S1#show ntp associations

    address         ref clock       st   when   poll reach  delay  offset   disp
    ~10.22.0.1       127.127.1.1      4      8     64    77  1.304 820.824  0.974
    sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

> S1(config)#ntp clock-period 999999


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/16.jpg "")

**S2**

> S2(config)#ntp server 10.22.0.1 source Gi0/0


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/17.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/18.jpg "")


> S2(config)#ntp clock-period 999999

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_32/19.jpg "")


