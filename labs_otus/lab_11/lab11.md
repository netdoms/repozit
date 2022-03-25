# Лабораторная работа N5
# Основы сетевой безопасности

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети |Шлюз по умолчанию |
|------|--------|-------|-------|-----|
| R1   | G0/0/0 | 192.168.1.1 |255.255.255.0     | —       |
| S1   |VLAN 1  |192.168.1.11  |255.255.255.0     | 192.168.1.1      |
| PC-A |NIC     |192.168.1.3 |255.255.255.0     | 192.168.1.1 |

**Задачи**

*Часть 1. Настройка основных параметров устройства*

*Часть 2. Настройка маршрутизатора для доступа по протоколу SSH*

*Часть 3. Настройка коммутатора для доступа по протоколу SSH*

*Часть 4. SSH через интерфейс командной строки (CLI) коммутатора*


> Router>enable 

> Router#configure terminal

> Router(config)#no ip domain-lookup

> Router(config)#hostname R1

> R1(config)# enable secret cisco

> R1(config)#line console 0

> R1(config-line)#password class

> R1(config-line)#exit

> R1(config)#service password-encryption

> R1(config)#banner motd #

Enter TEXT message.  End with the character '#'.

    Unauthorized access is strictly prohibited. #

> R1(config)#line vty 0 4

> R1(config-line)#password cisco

> R1(config-line)#login

> R1(config-line)#transport input all

> R1(config-line)#exit




> R1(config)#copy running-config startup-config

    Destination filename [startup-config]? 
    Building configuration...
    [OK]

  R1#reload  

**Настройте компьютер PC-A**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/2.jpg "")

