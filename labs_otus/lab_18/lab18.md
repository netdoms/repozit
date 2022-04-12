# Лабораторная работа N8
# Протоколы DHCPv4, SLAAC и DHCPv6 

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_18/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |
|------|--------|-------|-------|
| R1   | G0/0/0 |2001:db8:acad:2::1/64 |
|    |  |fe80::1  |
|   |G0/0/1 |2001:db8:acad:1::1/64 |
|   | |fe80::1 |
| R2  |G0/0/0  |2001:db8:acad:2::2/64|
|    | |fe80::2 |
|   |G0/0/1  |2001:db8:acad:3::1/64  |
|   | |fe80::1|
| PC-A  |NIC  |DHCP |
| PC-B   |NIC  |DHCP |

**Задачи**

*Часть 1. Создание сети и настройка основных параметров устройства*

*Часть 2. Проверка назначения адреса SLAAC от R1*

*Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1*

*Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1*

*Часть 5. Настройка и проверка DHCPv6 Relay на R2*

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

> S1(config)#interface range F0/2-4, F0/7-24, G0/1-2

> S1(config-if-range)#switchport mode access

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

> S1(config)#exit

> S1 copy running-config startup-config

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

