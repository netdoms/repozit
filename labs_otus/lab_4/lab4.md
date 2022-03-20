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

# Часть 2. Изучение таблицы МАС-адресов коммутатора

Шаг 1. Запишите МАС-адреса сетевых устройств

PC A

Packet Tracer PC Command Line 1.0

C:\>ipconfig /all

    FastEthernet0 Connection:(default port)

    Connection-specific DNS Suffix..: 
    Physical Address................: 0060.70E7.6EA1
    Link-local IPv6 Address.........: FE80::260:70FF:FEE7:6EA1
    IP Address......................: 192.168.1.1
    Subnet Mask.....................: 255.255.255.0
    Default Gateway.................: 0.0.0.0
    DNS Servers.....................: 0.0.0.0
    DHCP Servers....................: 0.0.0.0
    DHCPv6 Client DUID..............: 00-01-00-01-5D-DE-D0-45-00-60-70-E7-6E-A1

*PC B*

    Packet Tracer PC Command Line 1.0
> C:\>ipconfig /all

    FastEthernet0 Connection:(default port)

    Connection-specific DNS Suffix..: 
    Physical Address................: 00D0.BC28.37E9
    Link-local IPv6 Address.........: FE80::2D0:BCFF:FE28:37E9
    IP Address......................: 192.168.1.2
    Subnet Mask.....................: 255.255.255.0
    Default Gateway.................: 0.0.0.0
    DNS Servers.....................: 0.0.0.0
    DHCP Servers....................: 0.0.0.0
    DHCPv6 Client DUID..............: 00-01-00-01-E7-E8-5C-BE-00-D0-BC-28-37-E9

> S1#show interface F0/1

    FastEthernet0/1 is up, line protocol is up (connected)
    Hardware is Lance, address is 0002.1671.2701 (bia 0002.1671.2701)
    BW 100000 Kbit, DLY 1000 usec,
        reliability 255/255, txload 1/255, rxload 1/255
    Encapsulation ARPA, loopback not set
    Keepalive set (10 sec)
    Full-duplex, 100Mb/s
    input flow-control is off, output flow-control is off
    ARP type: ARPA, ARP Timeout 04:00:00
    Last input 00:00:08, output 00:00:05, output hang never
    Last clearing of "show interface" counters never
    Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0



S2
> #show interface f0/1

    FastEthernet0/1 is up, line protocol is up (connected)
    Hardware is Lance, address is 0001.4281.e201 (bia 0001.4281.e201)
    BW 100000 Kbit, DLY 1000 usec,
        reliability 255/255, txload 1/255, rxload 1/255
    Encapsulation ARPA, loopback not set
    Keepalive set (10 sec)
    Full-duplex, 100Mb/s
    input flow-control is off, output flow-control is off
    ARP type: ARPA, ARP Timeout 04:00:00
    Last input 00:00:08, output 00:00:05, output hang never
    Last clearing of "show interface" counters never
    Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0

МАС-адрес коммутатора S1 Fast Ethernet 0/1: 0002.1671.2701
МАС-адрес коммутатора S2 Fast Ethernet 0/1: 0001.4281.e201

### Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.

> S2#show mac address-table


            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----

    1    0002.1671.2701    DYNAMIC     Fa0/1

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/5.jpg "")

> S2#show mac address-table

            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----

    1    0002.1671.2701    DYNAMIC     Fa0/1
    1    0060.70e7.6ea1    DYNAMIC     Fa0/1
    1    00d0.bc28.37e9    DYNAMIC     Fa0/18

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_4/6.jpg "")

> S2#show mac address-table

            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----

    1    0002.1671.2701    DYNAMIC     Fa0/1
    1    0060.70e7.6ea1    DYNAMIC     Fa0/1
    1    00d0.bc28.37e9    DYNAMIC     Fa0/18

> S2#ping 192.168.1.11

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.1.11, timeout is 2 seconds:
    ..!!!
    Success rate is 60 percent (3/5), round-trip min/avg/max = 0/1/3 ms

> S2#show mac address-table

            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----

    1    0002.1671.2701    DYNAMIC     Fa0/1
    1    0003.e4aa.6d2a    DYNAMIC     Fa0/1
    1    0060.70e7.6ea1    DYNAMIC     Fa0/1
    1    00d0.bc28.37e9    DYNAMIC     Fa0/18

### Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов

Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода. Появились ли в таблице МАС-адресов новые адреса?
> НЕТ

> S2    S2# clear mac address-table dynamic

S2#show mac address-table


            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----

    1    0002.1671.2701    DYNAMIC     Fa0/1

    В таблице находится только mac S1 интерфейса Fast Ethernet 0/1

### Шаг 4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.



    Packet Tracer PC Command Line 1.0
    C:\>ping 192.168.1.1

    Pinging 192.168.1.1 with 32 bytes of data:

    Reply from 192.168.1.1: bytes=32 time=1ms TTL=128
    Reply from 192.168.1.1: bytes=32 time<1ms TTL=128
    Reply from 192.168.1.1: bytes=32 time=10ms TTL=128
    Reply from 192.168.1.1: bytes=32 time=4ms TTL=128

    Ping statistics for 192.168.1.1:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 10ms, Average = 3ms

    C:\>arp -a
    Internet Address      Physical Address      Type
    192.168.1.1           0060.70e7.6ea1        dynamic

    C:\>ping 192.168.1.11

    Pinging 192.168.1.11 with 32 bytes of data:

    Request timed out.
    Reply from 192.168.1.11: bytes=32 time=3ms TTL=255
    Reply from 192.168.1.11: bytes=32 time=3ms TTL=255
    Reply from 192.168.1.11: bytes=32 time=3ms TTL=255

    Ping statistics for 192.168.1.11:
        Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 3ms, Maximum = 3ms, Average = 3ms

    C:\>arp -a
    Internet Address      Physical Address      Type
    192.168.1.1           0060.70e7.6ea1        dynamic
    192.168.1.11          0003.e4aa.6d2a        dynamic

    C:\>ping 192.168.1.12

    Pinging 192.168.1.12 with 32 bytes of data:

    Request timed out.
    Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
    Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
    Reply from 192.168.1.12: bytes=32 time<1ms TTL=255

    Ping statistics for 192.168.1.12:
        Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.1.2

    Pinging 192.168.1.2 with 32 bytes of data:

    Reply from 192.168.1.2: bytes=32 time=5ms TTL=128
    Reply from 192.168.1.2: bytes=32 time=2ms TTL=128
    Reply from 192.168.1.2: bytes=32 time=4ms TTL=128
    Reply from 192.168.1.2: bytes=32 time=4ms TTL=128

    Ping statistics for 192.168.1.2:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 5ms, Average = 3ms

C:\>arp -a
        Internet Address      Physical Address      Type
        192.168.1.1           0060.70e7.6ea1        dynamic
        192.168.1.11          0003.e4aa.6d2a        dynamic
        192.168.1.12          0090.21e0.e191        dynamic

Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?

> Нет не появляется свой мак адрес.

В S2 появились все маки кроме S1

> S2#show mac address-table


        Vlan    Mac Address       Type        Ports
        ----    -----------       --------    -----

        1    0002.1671.2701    DYNAMIC     Fa0/1
        1    0060.70e7.6ea1    DYNAMIC     Fa0/1
        1    00d0.bc28.37e9    DYNAMIC     Fa0/18

### Вопрос для повторения

*Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть в крупных сетях?*

Чтобы не получить огромный broadcast домен. В современных IPv4 сетях широковещательный (broadcast) трафик является необходимым злом. Например, при помощи широковещательных запросов работает протокол ARP, операционная система Windows постоянно что-то рассылает в сеть, чтобы обнаружить другие компьютеры и т.п. Если мы подключим в одну сеть 65 тысяч устройств, то получится, что каждый квант времени кто-то что-то да отправит широковещательного. Такая сеть совершенно не сможет работать, потому что все будут заняты получением широковещательных пакетов.