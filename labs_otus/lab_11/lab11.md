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

**Настройка маршрутизатора**

> Router>enable 

> Router#configure terminal

> Router(config)#no ip domain-lookup

> Router(config)#hostname R1

> R1(config)#ip domain name test

> R1#crypto key generate rsa

> R1#show ssh

> R1(config)#show crypto key mypubkey rsa

    The name for the keys will be: R1.test
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.

    How many bits in the modulus [512]: 768
    % Generating 768 bit RSA keys, keys will be non-exportable...[OK]



> R1(config)#username admin privilege 1 password Adm1nP@55

> R1(config)#enable secret cisco

    *Mar 1 8:38:24.234: %SSH-5-ENABLED: SSH 1.99 has been enabled

> R1(config)#line console 0

> R1(config-line)#logging synchronous

> R1(config-line)#password class

> R1(config-line)#login

> R1(config-line)#exit

> R1(config)#service password-encryption

> R1(config)#interface gigabitethernet 0/0/1

    *Mar 1 0:3:24.328: %SSH-5-ENABLED: SSH 1.99 has been enabled

> R1(config-if)#ip address 192.168.1.1 255.255.255.0

> R1(config-if)#no shutdown

    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

> R1(config)#banner motd #

Enter TEXT message.  End with the character '#'.

    Unauthorized access is strictly prohibited. #

> R1(config)#line vty 0 4

> R1(config-line)#login local

> R1(config-line)#transport input all

> R1(config-line)#exit

> R1#copy running-config startup-config

    Destination filename [startup-config]? 
    Building configuration...
    [OK]

  R1#reload  

**Настройте компьютер PC-A**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/4.jpg "")


**Проверьте подключение к сети.**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/5.jpg "")

**Установите соединение с маршрутизатором по протоколу SSH**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/3.jpg "")




**Настройте коммутатора**

**Настройте основные параметры коммутатора.**

> Switch>enable 

> Switch#configure terminal

    Enter configuration commands, one per line.  End with CNTL/Z.

> Switch(config)#no ip domain-lookup

> Switch(config)#hostname S1

> S1(config)#ip domain name test

> S1(config)# enable secret cisco

> S1(config)#line console 0

> S1(config-line)#logging synchronous

> S1(config-line)#password class

> S1(config-line)#login

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

=Назначаем IP для  VLAN 1=

> S1(config)#interface vlan1

> S1(config-if)#ip address 192.168.1.11 255.255.255.0

> S1(config-if)#ip default-gateway 192.168.1.1

> S1(config-if)# no shutdown

    %LINK-5-CHANGED: Interface Vlan1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

> S1(config-if)# end

**Настройка коммутатора для соединения по протоколу SSH**

> S1(config)#ip domain name test

> S1#crypto key generate rsa

    The name for the keys will be: S1.test
    Choose the size of the key modulus in the range of 360 to 2048 for your
    General Purpose Keys. Choosing a key modulus greater than 512 may take
    a few minutes.

    How many bits in the modulus [512]: 768
    % Generating 768 bit RSA keys, keys will be non-exportable...[OK]

>S1(config)#line vty 0 4

>S1(config)#username admin privilege 1 password Adm1nP@55

    *Mar 1 1:8:2.468: %SSH-5-ENABLED: SSH 1.99 has been enabled

>S1(config-line)#login local

>S1(config-line)#end

**Сохраните текущую конфигурацию в файл загрузочной конфигурации.**

> S1#copy running-config startup-config

    Destination filename [startup-config]? 
    Building configuration...
    [OK]

>  S1#reload


**Установите соединение с маршрутизатором по протоколу SSH.**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_11/4.jpg "")

**Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора**

> S1#ssh ?

    -l  Log in using this user name
    -v  Specify SSH Protocol Version

> S1#ssh -l admin 192.168.1.1

    Password: 
    % Password:  timeout expired!
    % Login invalid

    [Connection to 192.168.1.1 closed by foreign host]
    S1#ssh -l admin 192.168.1.1

    Password: 


    Unauthorized access is strictly prohibited. 

    R1>
    R1>exit

    [Connection to 192.168.1.1 closed by foreign host]
    S1#

*Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?*

> S1#show ssh

        %No SSHv2 server connections running.
        %No SSHv1 server connections running

Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?

        Поднять  Vty более 1 + добавление пользователей в локальную базу сетевого устройства.