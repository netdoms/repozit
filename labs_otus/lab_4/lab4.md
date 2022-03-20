# Лабораторная работа N2

# Канальный уровень. Ethernet

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс  |IP-адрес     |
|----------|-----------|-------------|
|S1        |VLAN 1  |192.168.1.11 /24|
|S1        |VLAN 1  |192.168.1.12 /24|
|PC-A      |NIC     |192.168.1.1 /24|
|PC-B      |NIC     |192.168.1.2 /24|

**Цели**

**Часть 1.** Создание и настройка сети

**Часть 2.** Изучение таблицы МАС-адресов коммутатора

# Часть 1 Создание и настройка сети

Настроем узлы ПК PC-A и PC-B

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/2.jpg "")

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/3.jpg "")

* Выполните инициализацию и перезагрузку коммутаторов

> Switch>enable

> Switch#show flash

    Directory of flash:/

        1  -rw-     4414921          <no date>  c2960-lanbase-mz.122-25.FX.bin

    64016384 bytes total (59601463 bytes free)

> Switch#erase startup-config

    Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
    [OK]
    Erase of nvram: complete
    %SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram

> Switch#reload

% Please answer 'yes' or 'no'.

> System configuration has been modified. Save? [yes/no]: no
 
![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/4.jpg "")

Настройте базовые параметры коммутатора S1.

> Switch>enable

> Switch#conf t

    Enter configuration commands, one per line.  End with CNTL/Z.
> Switch(config)#hostname S1

> S1#conf t

> S1(config)#interface vlan1

> S1(config-if)#ip address 192.168.1.11 255.255.255.0

> S1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

> S1(config-if)#exit

> S1(config)#line con 0

> S1(config-line)#loggin synchronous 

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config-line)#line vty 0 4

> S1(config-line)#transport input all

> S1(config-line)#password cisco

> S1(config-line)#login

> S1(config-if)#exit

> S1(config)#enable secret class

> S1(config)# service password-encryption

Настройте базовые параметры коммутатора S2.

> Switch>enable

> Switch#conf t

    Enter configuration commands, one per line.  End with CNTL/Z.
> Switch(config)#hostname S2

> S2#conf t

> S2(config)#interface vlan1

> S2(config-if)#ip address 192.168.1.12 255.255.255.0

> S2(config-if)#no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

> S2(config-if)#exit

> S2(config)#line con 0

> S2(config-line)#loggin synchronous 

> S2(config-line)#password cisco

> S2(config-line)#login

> S2(config-line)#line vty 0 4

> S2(config-line)#transport input all

> S2(config-line)#password cisco

> S2(config-line)#login

> S2(config-if)#exit

> S2(config)#enable secret class

> S2(config)# service password-encryption

