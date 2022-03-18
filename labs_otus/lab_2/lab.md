


Таблица адресации

|Устройство|Интерфейс  |IP-адрес    |
|----------|-----------|------------|
|S1        |VLAN 1  |192.168.1.2 /24|
|PC-A      |NIC   |192.168.1.10 /24 |

Задачи

Часть 1. Проверка конфигурации коммутатора по умолчанию

Часть 2. Создание сети и настройка основных параметров устройства

Настройте базовые параметры коммутатора.
Настройте IP-адрес для ПК.

Часть 3. Проверка сетевых подключений
Отобразите конфигурацию устройства.

Протестируйте сквозное соединение, отправив эхо-запрос.

Протестируйте возможности удаленного управления с помощью Telnet

## Часть 1. Проверка конфигурации коммутатора по умолчанию

Подсоедините консольный кабель, как показано в топологии. На данном 

этапе не подключайте кабель Ethernet компьютера PC-A.

Подключаемся по консольному кабелю к коммутатору S1 

переходим в привелигированный режим.

Switch>enable 

Switch # show running-config

     configuration : 1078 bytes

    !

    version 12.2

    no service timestamps log datetime msec

    no service timestamps debug datetime msec

    no service password-encryption

    !

    hostname Switch

    !

    !

    !

    !

    !

    spanning-tree mode pvst

    spanning-tree extend system-id

    !

    interface FastEthernet0/1

    !

    interface FastEthernet0/2

    !

    interface FastEthernet0/3

    !

    interface FastEthernet0/4

    !

    interface FastEthernet0/5

    !

    interface FastEthernet0/6

    !

    interface FastEthernet0/7

    !

    interface FastEthernet0/8

    !

    interface FastEthernet0/9

    !

    interface FastEthernet0/10

    !

    interface FastEthernet0/11

    !

    interface FastEthernet0/12

    !

    interface FastEthernet0/13

    !

    interface FastEthernet0/14

    !
    interface FastEthernet0/15

    !
    interface FastEthernet0/16

    !
    interface FastEthernet0/17

    !
    interface FastEthernet0/18

    !
    interface FastEthernet0/19

    !

    interface FastEthernet0/20

    !

    interface FastEthernet0/21

    !

    interface FastEthernet0/22

    !

    interface FastEthernet0/23

    !

    interface FastEthernet0/24

    !

    interface GigabitEthernet0/1

    !

    interface GigabitEthernet0/2

    !

    interface Vlan1

    no ip address

    shutdown

    !

    !

    !

    !

    line con 0

    !

    line vty 0 4

    login

    line vty 5 15

    login

    !

    !

    !

    !

    end

Switch#show startup-config


    startup-config is not present


=В ОЗУ ничего нет=

Switch#show interface Vlan1

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Vlan1 is administratively down, line protocol is down

Hardware is CPU Interface, address is 0010.1158.4689 (bia 0010.1158.4689)

MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,

reliability 255/255, txload 1/255, rxload 1/255

Encapsulation ARPA, loopback not set

ARP type: ARPA, ARP Timeout 04:00:00

Last input 21:40:21, output never, output hang never

Last clearing of "show interface" counters never
  
Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0

Queueing strategy: fifo

Output queue: 0/40 (size/max)

5 minute input rate 0 bits/sec, 0 packets/sec

5 minute output rate 0 bits/sec, 0 packets/sec

1682 packets input, 530955 bytes, 0 no buffer

Received 0 broadcasts (0 IP multicast)

0 runts, 0 giants, 0 throttles

0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored

563859 packets output, 0 bytes, 0 underruns

0 output errors, 23 interface resets

0 output buffer failures, 0 output buffers swapped out

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Switch#show interface f0/6

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

FastEthernet0/6 is up, line protocol is up (connected)

Hardware is Lance, address is 00e0.a346.2e06 (bia 00e0.a346.2e06)

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

Queueing strategy: fifo

Output queue :0/40 (size/max)

5 minute input rate 0 bits/sec, 0 packets/sec

5 minute output rate 0 bits/sec, 0 packets/sec

956 packets input, 193351 bytes, 0 no buffer

Received 956 broadcasts, 0 runts, 0 giants, 0 throttles

0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort

0 watchdog, 0 multicast, 0 pause input

0 input packets with dribble condition detected

2357 packets output, 263570 bytes, 0 underruns

0 output errors, 0 collisions, 10 interface resets

0 babbles, 0 late collision, 0 deferred

0 lost carrier, 0 no carrier

0 output buffer failures, 0 output buffers swapped out

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Switch#show flash

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Directory of flash:/

1  -rw-     4414921          <no date>  c2960-lanbase-mz.122-25.
    
FX.bin

64016384 bytes total (59601463 bytes free)

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

## Часть 2 Настройка базовых параметров сетевых устройств

= Настраиваем основу =

Switch#clock set 15:05:00 17 Mar 2022

Switch#conf t

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

Enter configuration commands, one per line.  End with CNTL/Z.

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Switch(config)#no ip domain-lookup

Switch(config)#hostname S1

S1(config)#service password-encryption

S1(config)#banner motd #

Enter TEXT message.  End with the character '#'.

Unauthorized access is strictly prohibited. #

S1(config-if)# no shutdown

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

S1(config-if)#

%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

%IP-4-DUPADDR: Duplicate address 192.168.1.2 on Vlan1, sourced by 0005.5EE7.BBD9

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

=Назначаем IP для  VLAN 1=

S1(config)#interface vlan1

S1(config-if)#ip address 192.168.1.2 255.255.255.0

= Пароль на консоль =

S1(config)#line con 0

S1(config-line)#logging synchronous

S1(config-line)#

S1(config-line)#password cisco

S1(config-line)#login

= Настройте каналы виртуального соединения =

S1(config-if)#line vty 0 4

S1(config-line)#transport input all

### Настройте IP-адрес на компьютере PC-A.

# КАРТИНКА

### Часть 3. Проверка сетевых подключений

Отобразите конфигурацию устройства.

# КАРТИНКА

### Проверьте удаленное управление коммутатором S1

# КАРТИНКА

### Инициализация и перезагрузка коммутатора

S1#show flash

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

Directory of flash:/

1  -rw-     4414921          <no date>  c2960-lanbase-mz.122-25.FX.bin

64016384 bytes total (59601463 bytes free)

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

S1#copy running-config startup-config

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

Destination filename [startup-config]? 

Building configuration...

[OK]

&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&