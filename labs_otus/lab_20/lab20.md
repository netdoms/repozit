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

>R1(config-if)#exit

>R1# show ip interface brief

    *Jun  7 11:26:21.121: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun  7 11:26:22.128: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up


    *Jun  7 11:34:42.006: %SYS-5-CONFIG_I: Configured from console by console
    R1#show ip interface brief
    Interface                  IP-Address      OK? Method Status                Prot                                                                                       
    GigabitEthernet0/0         unassigned      YES unset  administratively down down                                                                                        
    GigabitEthernet0/1         192.168.10.1    YES manual up                    up                                                                                          
    GigabitEthernet0/2         unassigned      YES unset  administratively down down                                                                                        
    GigabitEthernet0/3         unassigned      YES unset  administratively down down                                                                                        
    Loopback0                  10.10.1.1       YES manual up                    up   


> R1#copy running-config startup-config

**Шаг Настройте базовые параметры каждого коммутатора**

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

> S2(config)#interface range Gi0/0, Gi0/3, Gi1/0-3

> S2(config-if-range)#switchport mode access

> S2(config-if-range)#shutdown

    *Jun  7 10:59:24.952: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
    *Jun  7 10:59:24.952: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  7 10:59:25.018: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  7 10:59:25.093: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    *Jun  7 10:59:25.179: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  7 10:59:25.227: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  7 10:59:25.952: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
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

> S1(config-if)#ip address 192.168.10.201 255.255.255.0

> S1(config-if)#ip default-gateway 192.168.10.1

> S1(config-if)#exit

> S1(config)#interface vlan10

> S1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


> S1(config)#vlan 10

> S1(config-vlan)#name Management

**VLAN  S1**

> S1(config)#vlan 333

> S1(config-vlan)#name native


> S1(config-vlan)#vlan 999

> S1(config-vlan)#name ParkingLot

**Транк S1 Gi0/1**
> S1(config-if)#interface Gi0/1

> S1(config-if)#switchport trunk encapsulation dot1q

> S1(config-if)#switchport mode trunk

> S1(config-if)#Switchport trunk native vlan 333

> S1(config-if)#Switchport trunk allowed vlan 10,999

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/4.jpg "")

**НАСТРОЙКА S2**   

> S2(config)#interface vlan10

S2(config)#name Management

> S2(config-if)#ip address 192.168.10.202 255.255.255.0

> S2(config-if)#ip default-gateway 192.168.10.1

 S2(config)#interface vlan10

> S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

> S2(config)#interface vlan 999

> S2(config-if)#no shutdown

> S2(config-vlan)#name ParkingLot

**Транк S2 Gi0/1**

> S2(config-vlan)#interface Gi0/1

> S2(config-if)#switchport trunk encapsulation dot1q

> S2(config-if)#switchport mode trunk

> S2(config-if)#Switchport trunk native vlan 333

> S2(config-if)#Switchport trunk allowed vlan 10,999

> S2(config-if)#Switchport trunk native vlan 333

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/5.jpg "")

# Отключить согласование DTP F0/1 на S1 и S2. #

> S1(config-if)#switchport mode access

> S1(config-if)#switchport nonegotiate

 S1#show interfaces Gi0/1 switchport | include Negotiation

    Negotiation of Trunking: Off

> S2(config-if)#switchport mode access

> S2(config-if)#switchport nonegotiate

> S2#show interfaces Gi0/1 switchport | include Negotiation

    Negotiation of Trunking: Off

# Настройка портов доступа S2 #

> S2(config-if)#interface Gi0/2

> S2(config-if)#switchport mode access

> S2(config-if)#Switchport trunk allowed vlan 10

S2(config)#vlan 999

> S2(config-vlan)#interface range Gi0/0, Gi0/3, Gi1/0-3

> S2(config-if-range)#switchport access  vlan 999

> S2(config-vlan)#exit

> S2(config)#interface range Gi0/0, Gi0/3, Gi1/0-3

> S2#show interfaces status

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/2.jpg "")

# Настройка портов доступа S1 #

> S1(config)#vlan 999

> S1(config-vlan)#interface range Gi0/3, Gi1/0-3

> S1(config-if-range)#switchport access  vlan 999

> S1(config-vlan)#shutdown

> S1(config-if-range)#exit

# Настройка портов доступа S1  Gi0/2 #

> S1(config-if)#interface Gi0/2

> S1(config-if)#switchport access  vlan 10

# Настройка портов доступа S1  Gi0/0 #

> S1(config-if)#interface Gi0/0

> S1(config-if)#switchport trunk encapsulation dot1q

> S1(config-if)#switchport mode access

> S1(config-if)#Switchport access native vlan 333

> S1(config-if)#Switchport access vlan 10

> S1(config-if)#Switchport access vlan 999

> S1#show interfaces status

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/3.jpg "")

# Конфигурация безопасности порта по умолчанию #

> S1# show port-security interface Gi0/2

|Функции|Настройки по умолчанию|
|---------------|-------------|
|Защита портов  |Disabled|
|Max MAC-адресов|1|
|Режим проверки на нарушение безопасности |Secure-down|
|Aging Time|0|
|Aging Type  |Absolute|
|Secure Static Address Aging|Disabled|
|Sticky MAC Address|0|

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/6.jpg "")

> S1(config)#interface Gi0/2

> S1(config-if)#switchport port-security

> S1(config-if)#switchport port-security maximum 3

> S1(config-if)#switchport port-security violation restrict

> S1(config-if)#switchport port-security aging time 60

> S1(config-if)#switchport port-security aging type inactivity

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/7.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/8.jpg "")

# Включите безопасность порта для F0 / 18 на S2 #

>S2(config)#interface Gi0/2

> S2(config-if)#switchport port-security

> S2(config-if)#switchport port-security maximum 2

> S2(config-if)#switchport port-security aging time 60

> S2(config-if)#switchport port-security violation protect

> S2(config-if)#switchport port-security aging type inactivity

> S2(config-if)#switchport port-security mac-address sticky

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/9.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/10.jpg "")

#  Реализовать безопасность DHCP snooping #

> S2(config)#ip dhcp snooping

> S2(config)#ip dhcp snooping vlan 10

> S2(config)#interface gi0/2

> S2(config-if)#ip dhcp snooping limit rate 5

> S2(config)#interface gi0/1

> S2(config-if)#ip dhcp snooping trust

> S2(config-if)#end

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/11.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/12.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/13.jpg "")

# Шаг 6. Реализация PortFast и BPDU Guard #

S2(config)#spanning-tree portfast  default

> S1(config)#spanning-tree portfast default

> S2(config)#interface gi0/2

> S2(config)#spanning-tree bpduguard enable 

> S2(config)#interface gi0/2

> S1(config)#panning-tree bpduguard enable

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/14.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/15.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/16.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/17.jpg "")


# Вопросы для повторения #

С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах, когда было сконфигурировано динамическое обучение - sticky?

MAC-адреса  привязываюся к рабочей конфигурации конфигурации автоматически

Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по неактивности?

Абсолютный - Защищенные адреса порта удаляются по истечении указанного времени устаревания. 

По таймеру неактивности -безопасные адреса на порту удаляются, только если они неактивны в течение указанного времени
