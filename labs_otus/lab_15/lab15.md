# Лабораторная работа N7
# Развертывание коммутируемой сети с резервными каналами

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/1.jpg "")

**Таблица адресации**

|Устройство|Интерфейс|IP адрес |Маска подсети |
|------|--------|-------|-------|
| S1   | VLAN 1 |192.168.1.1 |255.255.255.0     |
| S2   |VLAN 1  |192.168.1.2  |255.255.255.0     |
| S3   |VLAN 1  |192.168.1.3 |255.255.255.0     |

**Задачи**

*Создание сети и настройка основных параметров устройства*

*Выбор корневого моста*

*Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов*

*Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов*

**НАСТРОЙКА S1**

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



> S1(config)#interface vlan1

> S1(config-if)#ip address 192.168.1.1 255.255.255.0

> S1(config-if)#exit

> S1(config)#interface vlan1

> S1(config-if)#no shutdown

    *Jun  1 09:17:02.630: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
    *Jun  1 09:17:03.630: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

>S1(config-if)#exit   

>S1(config)#exit  

> S1#copy running-config startup-config

**НАСТРОЙКА S2**

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

> S2(config)#interface vlan1

> S2(config-if)#ip address 192.168.1.2 255.255.255.0

> S2(config-if)#exit

> S2(config)#interface vlan1

> S2(config-if)#no shutdown

    *Jun  1 09:34:29.894: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
    *Jun  1 09:34:30.894: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


> S2(config-if)#exit

> S2#copy running-config startup-config

**НАСТРОЙКА S3**

> Switch>enable 

> Switch#configure terminal

> Switch(config)#no ip domain-lookup

> Switch(config)#hostname S3

> S3(config)#line con 0

> S3(config-line)#logging synchronous

> S3(config-line)#password cisco

> S3(config-line)#login

> S3(config)#line vty 0 4

> S3(config-line)#password cisco

> S3(config-line)#login

> S3(config-line)#transport input all

> S3(config-line)#exit

> S3(config)#banner motd #

    Enter TEXT message.  End with the character '#'.

> Unauthorized access is strictly prohibited. # 

> S3(config)#enable secret class

> S3(config)#service password-encryption

> S3(config)#interface vlan1

> S3(config-if)#ip address 192.168.1.3 255.255.255.0

> S3(config-if)#exit

> S3(config)#interface vlan1

> S3(config-if)#no shutdown

    *Jun  1 09:34:29.894: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
    *Jun  1 09:34:30.894: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


> S3(config-if)#exit

> S3(config)#exit

> S3#copy running-config startup-config

**ПРОВЕРКА СВЯЗИ**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/2.jpg "")


![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/3.jpg "")

**Определение корневого моста**

**S1 Включение порты Gi0/1 и Gi0/3.**

> S1(config)#interface range Gi0/0-3, Gi1/0-3

> S1(config-if-range)#shutdown

> S1(config)#switchport trunk encapsulation dot1q

> S1(config)#switchport mode trunk


    *Jun  1 11:51:35.528: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
    *Jun  1 11:51:35.560: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down
    *Jun  1 11:51:35.591: %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    *Jun  1 11:51:35.621: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  1 11:51:35.652: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  1 11:51:35.682: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    S1(config-if-range)#
    *Jun  1 11:51:35.712: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  1 11:51:35.820: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  1 11:51:36.528: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to down
    *Jun  1 11:51:36.560: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
    *Jun  1 11:51:36.591: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
    *Jun  1 11:51:36.621: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S1(config-if-range)#
    *Jun  1 11:51:36.652: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun  1 11:51:36.682: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun  1 11:51:36.712: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun  1 11:51:36.893: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down

> S1(config)#interface Gi0/1

> S1(config-if)#no shutdown

    *Jun  1 13:12:36.625: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun  1 13:12:37.625: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

> S1(config-if)#exit

> S1(config)#interface Gi0/3

> S1(config-if)#no shutdown

    *Jun  1 13:12:36.625: %LINK-3-UPDOWN: Interface GigabitEthernet0/3, changed state to up
    *Jun  1 13:12:37.625: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to up



**S2 Включение порты Gi0/1 и Gi0/3.**

> S2(config)#interface range Gi0/0-3, Gi1/0-3

> S2(config)#shutdown

> S2(config)#switchport trunk encapsulation dot1q

> S2(config)#switchport mode trunk


> S2(config-if-range)#switchport mode trunk

    *Jun  1 11:31:23.921: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
    *Jun  1 11:31:23.954: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down
    *Jun  1 11:31:23.986: %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    *Jun  1 11:31:24.017: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  1 11:31:24.048: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  1 11:31:24.078: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    S2(config-if-range)#
    *Jun  1 11:31:24.108: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  1 11:31:24.225: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  1 11:31:24.921: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to down
    *Jun  1 11:31:24.954: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
    *Jun  1 11:31:24.987: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
    *Jun  1 11:31:25.018: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S2(config-if-range)#
    *Jun  1 11:31:25.048: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun  1 11:31:25.078: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun  1 11:31:25.108: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun  1 11:31:25.299: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down

> S2(config-if-range)#exit  

> S2(config)#interface Gi0/1

> S2(config)#no shutdown

    *Jun  1 13:07:43.074: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    *Jun  1 13:07:44.074: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

> S2(config-if-range)#exit

> S2(config)#interface Gi0/3

> S2(config-if)#no shutdown

    *Jun  1 13:00:21.539: %LINK-3-UPDOWN: Interface GigabitEthernet0/3, changed state to up
    S3(config-if)#
    *Jun  1 13:00:22.539: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to up



**S3 Включение порты Gi0/1 и Gi0/3.**

> S3(config)#interface range Gi0/0-3, Gi1/0-3

> S3(config)#shutdown


    *Jun  1 12:47:56.186: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to administratively down
    *Jun  1 12:47:56.219: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down
    *Jun  1 12:47:56.250: %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
    *Jun  1 12:47:56.281: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed state to administratively down
    *Jun  1 12:47:56.311: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed state to administratively down
    *Jun  1 12:47:56.341: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed state to administratively down
    S3(config-if-range)#
    *Jun  1 12:47:56.372: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed state to administratively down
    *Jun  1 12:47:56.412: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed state to administratively down
    *Jun  1 12:47:57.186: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to down
    *Jun  1 12:47:57.219: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
    *Jun  1 12:47:57.250: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
    *Jun  1 12:47:57.281: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to down
    S3(config-if-range)#
    *Jun  1 12:47:57.311: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0, changed state to down
    *Jun  1 12:47:57.341: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/1, changed state to down
    *Jun  1 12:47:57.372: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/2, changed state to down
    *Jun  1 12:47:57.412: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/3, changed state to down



> S3(config)#switchport trunk encapsulation dot1q

> S3(config)#switchport mode trunk

> S3(config)#exit

> S3(config)#interface Gi0/1

> S3(config)#no shutdown

    *Jun  1 12:56:59.963: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
    S3(config-if)#
    *Jun  1 12:57:00.963: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up


> S3(config-if)#exit

> S3(config)#interface Gi0/3

> S3(config-if)#no shutdown

    *Jun  1 13:00:21.539: %LINK-3-UPDOWN: Interface GigabitEthernet0/3, changed state to up
    S3(config-if)#
    *Jun  1 13:00:22.539: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/3, changed state to up


> S1#show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/4.jpg "")

> S2#show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/5.jpg "")

> S3#show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/6.jpg "")




![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/7.jpg "")

Какой коммутатор является корневым мостом? S3

Какие порты на коммутаторе являются корневыми портами? root

Какие порты на коммутаторе являются назначенными портами? Desg 

Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? 

S1 Gi0/0

Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?

На всех мостах блокируются все порты, не являющиеся корневыми и назначенными.

**Часть 3:	Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов**

> S3(config)#interface Gi0/1


> S3(config-if)#spanning-tree cost 3

>S3# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/9.jpg "")

>S2# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/10.jpg "")

>S1# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/11.jpg "")


**Шаг 4:	Удалите изменения стоимости порта.**

> S3(config)#interface Gi0/1

> S3(config-if)#no spanning-tree cost 3

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/12.jpg "")

**Часть 4:	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов**

**Gi0/0  Gi0/2 включение**


> S1(config)#interface gi0/0

> S1(config-if)#no shutdown

    *Jun  1 17:11:43.527: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
    *Jun  1 17:11:44.527: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

> S1(config-if)#exit

> S1(config)#interface gi0/2

> S1(config-if)#no shutdown

    *Jun  1 17:11:43.527: %LINK-3-UPDOWN: Interface GigabitEthernet0/2, changed state to up
    *Jun  1 17:13:11.790: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

> S2(config)#interface gi0/0

> S2(config-if)#no shutdown

    *Jun  1 17:11:43.527: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
    *Jun  1 17:11:44.527: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

> S2(config-if)#exit

> S2(config)#interface gi0/2

> S2(config-if)#no shutdown

    *Jun  1 17:11:43.527: %LINK-3-UPDOWN: Interface GigabitEthernet0/2, changed state to up
    *Jun  1 17:13:11.790: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up


> S3(config)#interface gi0/0

> S3(config-if)#no shutdown

    *Jun  1 17:11:43.527: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
    *Jun  1 17:11:44.527: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

> S3(config-if)#exit

> S3(config)#interface gi0/2

> S3(config-if)#no shutdown

    *Jun  1 17:21:07.451: %LINK-3-UPDOWN: Interface GigabitEthernet0/2, changed state to up
    *Jun  1 17:21:08.451: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

>S3# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/13.jpg "")

>S2# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/14.jpg "")

>S1# show spanning-tree

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_15/15.jpg "")


Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого моста

Порт с Bridge ID  Priority меньшим значением

Почему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах?

Bridge ID  Priority меньшим значением

	Вопросы для повторения

    1.	Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта?

    Cost порта

    2.	Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP при выборе порта?

    Bridge ID  Priority меньшим значением


    3.	Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP при выборе порта?

     меньшим значением порта
