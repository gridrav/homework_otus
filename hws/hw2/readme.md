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

+Подключимся к консоли коммутатора S1 через консольный кабель и выполним базовую настройку

```
Switch>
Switch>
Switch>en
Switch#conf t
Switch(config)#no ip domain-lookup
Switch(config)#hostname S1
S1(config)#exit
```

+Перезагрузим коммутатор S1 командой reload.

+Подключимся к консоли коммутатора S2 через консольный кабель и выполним базовую настройку
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
![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-a all.png)
![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/pc-b all.png)

Назовите физические адреса адаптера Ethernet.

MAC-адрес компьютера PC-A: Physical Address................: 000C.CF8C.0A48

MAC-адрес компьютера PC-B: Physical Address................: 00D0.BC07.ACD4

b)Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s1_interface f01.png)

Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).

МАС-адрес коммутатора S1 Fast Ethernet 0/1:

> Hardware is Lance, address is 000b.be31.c201 (bia 000b.be31.c201)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw2/Screens/s2_interface f01.png)

МАС-адрес коммутатора S2 Fast Ethernet 0/1:

> Hardware is Lance, address is 0001.64c0.8501 (bia 0001.64c0.8501)

##### Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.

Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.

a)Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.

Откройте окно конфигурации

b)В привилегированном режиме EXEC введите команду show mac address-table и нажмите клавишу ввода.

S2# show mac address-table

Даже если сетевая коммуникация в сети не происходила (т. е. если команда ping не отправлялась), коммутатор может узнать МАС-адреса при подключении к ПК и другим коммутаторам.

Вопросы:

1.Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?

Какие МАС-адреса записаны в таблице? С какими портами коммутатора они сопоставлены и каким устройствам принадлежат? Игнорируйте МАС-адреса, сопоставленные с центральным процессором.

Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды show mac address-table? Работает ли это решение в любой ситуации?

##### Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.

a) В привилегированном режиме EXEC введите команду clear mac address-table dynamic и нажмите клавишу Enter.

S2# clear mac address-table dynamic

b) Снова быстро введите команду show mac address-table.

Вопросы:

Указаны ли в таблице МАС-адресов адреса для VLAN 1? Указаны ли другие МАС-адреса?

Через 10 секунд введите команду show mac address-table и нажмите клавишу ввода. Появились ли в таблице МАС-адресов новые адреса?

##### Шаг 4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.

a) На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

Откройте командную строку.

Вопрос:

Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?

b)Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.

Вопрос:

От всех ли устройств получены ответы? Если нет, проверьте кабели и IP-конфигурации.

Закройте командную строку.
               
c)Подключившись через консоль к коммутатору S2, введите команду show mac address-table.

Откройте окно конфигурации

Вопрос:

Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса? Если да, то какие адреса и устройства?

На компьютере PC-B откройте командную строку и еще раз введите команду arp -a.

Вопрос:

Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?
