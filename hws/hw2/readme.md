#  Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

###  Топология:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/2.png)

###  Цели:
       
Часть 1. Создание и настройка сети

Часть 2. Изучение таблицы МАС-адресов коммутатора

###  Таблица адресации

| Устройство       | Интерфейс      | IP-адрес                 | Маска подсети            | 
|-----------------:|:---------------|-------------------------:|:-------------------------|
| S1               | VLAN 1         | 192.168.1.11             |  255.255.255.0           | 
| S1               | VLAN 1         | 192.168.1.12             |  255.255.255.0           | 
| PC-A             | NIC            | 192.168.1.1              |  255.255.255.0           | 
| PC-B             | NIC            | 192.168.1.2              |  255.255.255.0           | 


###  Решение:

#### Часть 1. Создание и настройка сети
##### Шаг 1. Подключите сеть в соответствии с топологией.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/network.png)

##### Шаг 2. Настройте узлы ПК.

Для PC-A задам статический IP-адрес и маску подсети.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-a.png)

Для PC-B

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b.png)

##### Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.

+ Подключимся к консоли коммутатора S1 через консольный кабель и выполним базовую настройку

```
Switch>
Switch>
Switch>en
Switch#conf t
Switch(config)#no ip domain-lookup
Switch(config)#hostname S1
S1(config)#exit
```

+ Перезагрузим коммутатор S1 командой reload.

+ Подключимся к консоли коммутатора S2 через консольный кабель и выполним базовую настройку
```
Switch>
Switch>
Switch>en
Switch#conf t
Switch(config)#no ip domain-lookup
Switch(config)#hostname S2
S2(config)#exit
```
##### Шаг 4. Настройте базовые параметры каждого коммутатора.

+ Настройте имена устройств в соответствии с топологией.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/name.png)

+ Настройте IP-адреса, как указано в таблице адресации.

```
S1(config)#interface vlan 1
S1(config-if)#ip ad
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no shutdown
```
и

```
S2(config)#interface vlan 1
S2(config-if)#ip ad
S2(config-if)#ip address 192.168.1.12 255.255.255.0
S2(config-if)#no shutdown
```

+ Назначьте cisco в качестве паролей консоли и VTY.

```
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#line con
S1(config)#line console 0
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
S1#
S1#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#en
S1(config)#enable secret cisco
S1(config)#
S1(config)#exit
```

и

```
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#line console 0
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#end
S2#
S2#configure t
S2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#enable secret cisco
S2(config)#exit
S2#
```

+ Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.

```
S1(config)#service password-encryption 
S1(config)#enable secret class
```

и

```
S2(config)#service password-encryption 
S2(config)#enable secret class
```

#### Часть 2. Изучение таблицы МАС-адресов коммутатора
Как только между сетевыми устройствами начинается передача данных, коммутатор выясняет МАС-адреса и строит таблицу.

##### Шаг 1. Запишите МАС-адреса сетевых устройств.

a)Откройте командную строку на PC-A и PC-B и введите команду ipconfig /all.
![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-a_all.png)
![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b_all.png)

Назовите физические адреса адаптера Ethernet.

MAC-адрес компьютера PC-A: Physical Address................: 000C.CF8C.0A48

MAC-адрес компьютера PC-B: Physical Address................: 00D0.BC07.ACD4

b)Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s1_interfacef01.png)

Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).

МАС-адрес коммутатора S1 Fast Ethernet 0/1:

> Hardware is Lance, address is 000b.be31.c201 (bia 000b.be31.c201)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s2_interfacef01.png)

МАС-адрес коммутатора S2 Fast Ethernet 0/1:

> Hardware is Lance, address is 0001.64c0.8501 (bia 0001.64c0.8501)

##### Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.

Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s2_showmac.png)

Даже если сетевая коммуникация в сети не происходила (т. е. если команда ping не отправлялась), коммутатор может узнать МАС-адреса при подключении к ПК и другим коммутаторам.

Вопросы:

Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?

> Записан МАС-адреса коммутатора S1: 000b.be31.c201    DYNAMIC     

С какими портами коммутатора они сопоставлены и каким устройствам принадлежат? 

> Fa0/1

##### Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s2_showmacclear.png) 

Указаны ли в таблице МАС-адресов адреса для VLAN 1? Указаны ли другие МАС-адреса?

> Не указаны

Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода. Появились ли в таблице МАС-адресов новые адреса?

> Появился вновь адрес S1 ( 1    000b.be31.c201    DYNAMIC     Fa0/1)

##### Шаг 4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.

a) На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

```
C:\>arp -a
No ARP Entries Found
```

После отправки ping, команда arp работает

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b_arp.png) 

Вопрос:

Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?

> не получено

b)Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.

Вопрос:

От всех ли устройств получены ответы? Если нет, проверьте кабели и IP-конфигурации.

> От всех

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b_testping.png)
               
c)Подключившись через консоль к коммутатору S2, введите команду show mac address-table.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s2_console.png)

Вопрос:

Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса? Если да, то какие адреса и устройства?

> Да, добавил 2 устройства

   1    00d0.bc07.acd4    DYNAMIC     Fa0/18   - MAC-адрес компьютера PC-B
   
   1    00d0.ffa9.1572    DYNAMIC     Fa0/1    - ???

На компьютере PC-B после повторного ввода команды arp -a.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b_arp2.png)

Вопрос:

Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?

> Появились

Сохраненный файл cisco packet tracer [lab2.pkt]

[lab2.pkt]:https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/lab2.pkt
