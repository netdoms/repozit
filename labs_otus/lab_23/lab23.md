# Лабораторная работа N20
# Принципы обеспечения безопасности сети

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_20/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети|
|------|-----------|-----------|-------------|
| R1   | G0/0/1    | 10.53.0.1 |255.255.255.0|
|      |Loopback 1 |172.16.1.1 |255.255.255.0|
| R2   | G0/0/1    |10.53.0.2  |255.255.255.0|
|      | Loopback1 |192.168.1.1|255.255.255.0|

copy running-config startup-config




**Задачи**

*Создание сети и настройка основных параметров устройства*

*Настройка и проверка базовой работы протокола  OSPFv2 для одной области*

*Оптимизация и проверка конфигурации OSPFv2 для одной области*



**Часть 1. Создание сети и настройка основных параметров устройства**

**R1**

R1(config)# interface G0/0/1

R1(config-if)#ip address 10.53.0.1 255.255.255.0

R1(config-if)#no shutdown

R1(config-if)#

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#exit

R1(config)#interface Loopback1

R1(config-if)#

    %LINK-5-CHANGED: Interface Loopback1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R1(config-if)#ip address 172.16.1.1  255.255.255.0

R1(config-if)#exit

R1(config)#end

**R2**
R2(config)# interface G0/0/1

R2(config-if)#ip address 10.53.0.2 255.255.255.0

R2(config-if)#no shutdown

R2(config-if)#

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R2(config-if)#exit

R2(config)#interface Loopback1

R2(config-if)#

    %LINK-5-CHANGED: Interface Loopback1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R2(config-if)#ip address 192.168.1.1  255.255.255.0

R2(config-if)#exit

R2(config)#end




**Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области**
