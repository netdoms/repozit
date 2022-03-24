# Лабораторная работа N4
# IPv6 адресация

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_8/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IPv6-адрес |Длина префикса |Шлюз по умолчанию |
|------|--------|-------|-------|-----|
| R1   | G0/0/0 | 2001:db8:acad: a::1 |64     | —       |
|      |G0/0/1  |2001:db8:acad:1::1  |64     | —        |
| S1   |VLAN 1  |2001:db8:acad:1::b  |64     | —        |
| PC-A |NIC     |2001:db8:acad:1::3  |64     | fe80::1  |
| PC-B |NIC     |2001:db8:acad: a::3  |64     | fe80::1 |

**Задачи**
*Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора*

*Часть 2. Ручная настройка IPv6-адресов*

*Часть 3. Проверка сквозного соединения*

**Необходимые ресурсы**

*1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)*

*1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)*

*2 ПК (Windows и программа эмуляции терминала, такая как Tera Term)
Консольные кабели для настройки устройств Cisco IOS через консольные порты.*

**Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора**






**Настройте маршрутизатор**

> Router>enable 

> Router#configure terminal

> Router(config)#hostname S1

> S1(config)# enable secret cisco

> S1(config)#line console 0

> S1(config-line)#password class

> S1(config-line)#exit

> S1(config)#service password-encryption

> S1(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

Unauthorized access is strictly prohibited. #

> S1(config)#line vty 0 4

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config-line)#transport input all

> S1(config-line)#exit

> S1(config)#copy running-config startup-config

    Destination filename [startup-config]? 
    Building configuration...
    [OK]

**Настройте коммутатор**

> Switch>enable

> Switch#show sdm prefer

    The current template is "default" template.
    The selected template optimizes the resources in
    the switch to support this level of features for
    0 routed interfaces and 1024 VLANs.

    number of unicast mac addresses:                  8K
    number of IPv4 IGMP groups + multicast routes:    0.25K
    number of IPv4 unicast routes:                    0
    number of IPv6 multicast groups:                  0
    number of directly-connected IPv6 addresses:      0
    number of indirect IPv6 unicast routes:           0
    number of IPv4 policy based routing aces:         0
    number of IPv4/MAC qos aces:                      0.125k
    number of IPv4/MAC security aces:                 0.375k
    number of IPv6 policy based routing aces:         0
    number of IPv6 qos aces:                          20
    number of IPv6 security aces:                     25


> Switch#configure terminal

> Switch(config)#hostname S1

> S1(config)#enable secret class

> S1(config)#service password-encryption

> S1(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. #

> S1(config)#sdm prefer dual-ipv4-and-ipv6 default 

    Changes to the running SDM preferences have been stored, but cannot take effect until the next reload.
    Use 'show sdm prefer' to see what SDM preference is currently active.
    Switch(config)#end

> S1#copy running-config startup-config

    Destination filename [startup-config]? 

        Building configuration...

        [OK]

> S1#reload





**Ручная настройка IPv6-адресов**

***Назначьте IPv6-адреса интерфейсам Ethernet на R1***



***Активируйте IPv6-маршрутизацию на R1***

***Назначьте IPv6-адреса интерфейсу управления (SVI) на S1***
> S1(config)# interface gigabitethernet 0/0/0

> S1(config-if)# ipv6 address 2001:db8:acad:a::1/64

> S1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

> S1(config-if)#exit

> S1(config)# interface gigabitethernet 0/0/1

> S1(config-if)#ipv6 address 2001:db8:acad:1::1/64

> S1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up


***Назначьте компьютерам статические IPv6-адреса***



**Проверка сквозного подключения**

**Вопросы для повторения**

*Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?*

*Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?*
