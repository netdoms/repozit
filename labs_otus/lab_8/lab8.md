# Лабораторная работа N4
# IPv6 адресация

**Топология**

![](https://github.com/netdoms/repozit/blob/main/labs_otus/lab_2/1.jpg "")

**Таблица адресации**




|Устройство |Интерфейс|IPv6-адрес |Длина префикса |Шлюз по умолчанию |
|------|--------|-------|-------|-----|
| R1   | G0/0/0 | 2001:db8:acad:a::1 |64     | —        |
|      |G0/0/1  |2001:db8:acad:1::1  |64     | —        |
| S1   |VLAN 1  |2001:db8:acad:1::b  |64     | —        |
| PC-A |NIC     |2001:db8:acad:1::3  |64     | fe80::1  |
| PC-B |NIC     |2001:db8:acad:a::3  |64     | fe80::1  |

**Задачи*
*Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

*Часть 2. Ручная настройка IPv6-адресов

*Часть 3. Проверка сквозного соединения

**Необходимые ресурсы**

1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным)

1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)

2 ПК (Windows и программа эмуляции терминала, такая как Tera Term)
Консольные кабели для настройки устройств Cisco IOS через консольные порты.




