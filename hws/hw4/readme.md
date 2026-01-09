#  Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах 

###  Топология:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/top1.png)

###     Таблица адресации:

| Устройство       | Интерфейс      | IPv6-адрес               |Link local IPv6-адрес     |   Длина префикса     |       Шлюз по умолчанию  | 
|:-----------------|:---------------|:-------------------------|:------------------------|:---------------------|:-------------------------|
| R1               | G0/0/0         | 2001:db8:acad:a::1       |  fe80::1                |  64                  |  -                       | 
| R1               | G0/0/1         | 2001:db8:acad:1::1       |  fe80::1                |  64                  |  -                       | 
| S1               | VLAN 1         | 2001:db8:acad:1::b       |  fe80::b                |  64                  |  -                       | 
| PC-A             | NIC            | 2001:db8:acad:1::3       | SLACC                   |  64                  | fe80::1                  |     
| PC-B             | NIC            | 2001:db8:acad:a::3       | SLACC                   |  64                  | fe80::1                  |  

### Задачи

Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

Часть 2. Ручная настройка IPv6-адресов

Часть 3. Проверка сквозного соединения

###  Решение:

#### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

После подключения сети, инициализации и перезагрузки маршрутизатора и коммутатора выполните следующие действия:
          
##### Шаг 1. Настройте маршрутизатор.

Назначьте имя хоста и настройте основные параметры устройства.

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret cisco
R1(config)#line console 0
R1(config-line)#pas
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#
R1(config)#banner motd #USE R1#
R1(config)#exit
```

##### Шаг 2. Настройте коммутатор.

Назначьте имя хоста и настройте основные параметры устройства.

```
Switch#conf t
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable secret cisco
S1(config)#line console 0
S1(config-line)#pas
S1(config-line)#password cisco
S1(config-line)#login
S1(config)#exit
S1(config)#banner motd #USE S1#
```

#### Часть 2. Ручная настройка IPv6-адресов

##### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1.

+ Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.
  
```
R1(config)# interface GigabitEthernet0/0/0
R1(config-if)# ipv6 address 2001:db8:acad:a::1/64
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
```

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_1.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_2.png)

+ Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_3.png)

+ Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.
  
```
R1(config)# interface GigabitEthernet0/0/0
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# exit
```

+ Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на fe80::1.

```
R1# show ipv6 interface g0/0/0
R1# show ipv6 interface g0/0/1
```

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_4.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_5.png)

Какие группы многоадресной рассылки назначены интерфейсу G0/0?

< FF02::1

##### Шаг 2. Активируйте IPv6-маршрутизацию на R1.

+ В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_6.png)

Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?

< Нет, до включения IPv6-маршрутизации на R1 PC-B получит только link-local адрес FE80:: ...

+ Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_7.png)

+ Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_8.png)


![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_9.png)

Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1?

PC-B получит глобальный префикс маршрутизации 2001:db8:acad:a::/64 от R1 только в том случае, если он настроен на автоматическое получение адреса (SLAAC). При статической настройке IPv6-адреса (2001:db8:acad:a::3), как указано в таблице адресации, компьютер использует заданный вручную адрес и не изменяет его при получении RA-сообщений от маршрутизатора.

##### Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.

+ Назначьте адрес IPv6 для S1. Также назначьте этому интерфейсу локальный адрес канала fe80::b.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_10.png)
  
+ Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan 1.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_11.png)

##### Шаг 4. Назначьте компьютерам статические IPv6-адреса.

+ Откройте окно Свойства Ethernet для каждого ПК и назначьте адресацию IPv6.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_12.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_13.png)
  
Убедитесь, что оба компьютера имеют правильную информацию адреса IPv6

#### Часть 3. Проверка сквозного подключения

С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_14.png)

Отправьте эхо-запрос на интерфейс управления S1 с PC-A.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_15.png)

Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_16.png)

С PC-B отправьте эхо-запрос на PC-A.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_17.png)

С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/Screenshot_18.png)

Примечание.  В случае отсутствия сквозного подключения проверьте, правильно ли указаны IPv6-адреса на всех устройствах.

Сохраненный файл cisco packet tracer [lab4.pkt]

[lab4.pkt]:https://github.com/gridrav/homework_otus/blob/main/hws/hw4/Screens/lab4.pkt
