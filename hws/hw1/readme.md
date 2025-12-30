#  Лабораторная работа. Базовая настройка коммутатора

###  Топология:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hw1/Screens/1.png?raw=true)

###  Задание:

1. Проверка конфигурации коммутатора по умолчанию.
2. Создание сети и настройка основных параметров устройства.
+ Настройте базовые параметры коммутатора.
+ Настройте IP-адрес для ПК.
4. Проверка сетевых подключений.
+ Отобразите конфигурацию устройства.
+ Протестируйте сквозное соединение, отправив эхо-запрос.
+ Протестируйте возможности удаленного управления с помощью Telnet.

###  Таблица адресации

| Устройство       | Интерфейс      | IP-адрес / префикс       | 
|-----------------:|:---------------|-------------------------:|
| S1               | VLAN 1         | 192.168.1.2 /24          | 
| PC-A             | NIC            | 192.168.1.10 /24         | 

###  Решение:

#### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию.
##### Шаг 1. Создаем сеть согласно топологии.
Устанавливаем консольное подключение

<img width="1100" height="736" alt="image" src="https://github.com/user-attachments/assets/253704b4-e61f-43a7-91ec-cedb0a3f68bf" />

Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?

Консольное подключение нужно использовать для первоначальной настройки коммутатора, так как оно обеспечивает прямой доступ к интерфейсу конфигурации устройства. Протоколы Telnet и SSH (Secure Shell) не подходят для этого этапа, так как требуют функционирующей сети и не обеспечивают безопасную настройку. 

##### Шаг 2. Проверяем настройки коммутатора по умолчанию.

`Switch#show running-config`

```
Building configuration...

Current configuration : 1080 bytes
!
version 15.0
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
```
+ Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?

    24

+ Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?

    2

+ Каков диапазон значений, отображаемых в vty-линиях?

    От 0 до 4 и от 5 до 15

+ Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).
  ```
  Switch#show startup-config
  startup-config is not present
  ```
  Означает, что файл загрузочной конфигурации отсутствует, используются базовые настройки.
+ Назначен ли IP-адрес сети VLAN 1? Какой MAC-адрес имеет SVI? Данный интерфейс включен?
  Посмотрим вывод команды show interfaces, IP-адрес сети VLAN 1 не назначен. MAC-адрес для SVI 0004.9a2b.e7d1. Данный интерфейс выключен.
+ Изучите IP-свойства интерфейса SVI сети VLAN 1. Какие выходные данные вы видите?
  0 IP multicast
+ Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.
 ```
 Vlan1 is administratively down, line protocol is down
  Hardware is CPU Interface, address is 0004.9a2b.e7d1 (bia 0004.9a2b.e7d1)
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
  ```
+ Под управлением какой версии ОС Cisco IOS работает коммутатор?
  show version -> Version 15.0(2)SE4
+ Как называется файл образа системы?
  flash:c2960-lanbasek9-mz.150-2.SE4.bin
+ Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.
  ```
  Switch#show interface f0/6 
  FastEthernet0/6 is up, line protocol is up (connected)
  ```
  Интерфейс включен.
  Какой MAC-адрес у интерфейса?
  0001.42c3.4a06
  Какие настройки скорости и дуплекса заданы в интерфейсе?
  Full-duplex, 100Mb/s
+ Изучите флеш-память.
  Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.
  ```
  Switch#show flash
  Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

  64016384 bytes total (59345929 bytes free)
  ```
  В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.
  Вопрос:
  Какое имя присвоено образу Cisco IOS?
  2960-lanbasek9-mz.150-2.SE4
#### Часть 2.Настройка базовых параметров сетевых устройств
##### Шаг 1. Настроим базовые параметры коммутатора.
```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip d
Switch(config)#no ip do
Switch(config)#no ip dom
Switch(config)#no ip domain
Switch(config)#no ip domain-
Switch(config)#no ip domain-lookup
Switch(config)#hostname S1
S1(config)#service password-encryption
S1(config)#enable secret class
S1(config)#banner motd #
Enter TEXT message.  End with the character '#'.
Unauthorized access is strictly prohibited. #

S1(config)#
```
+ Назначьте IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому получим возможность удаленного управления коммутатором.
 Согласно конфигурации по умолчанию коммутатором можно управлять через VLAN 1.
```
S1#
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#
S1(config)#
S1(config)#in
S1(config)#interface vlan 1
S1(config-if)#ip ad
S1(config-if)#ip address 192.168.1.2 255.255.255.0
S1(config-if)#no sh
S1(config-if)#no shutdown 

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

S1(config-if)#
```
+ Доступ через порт консоли также следует ограничить  с помощью пароля. Используйте cisco в качестве пароля для входа в консоль. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используйте параметр logging synchronous.
  
```
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#logging synchronous 
S1(config-line)#
```

+ Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

```
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#transport input telnet
S1(config-line)#
```

Для чего нужна команда login? Для того, чтобы применился пароль.

##### Шаг 2. Настроим IP-адрес на компьютере PC-A.
Назначьте компьютеру IP-адрес и маску подсети в соответствии с таблицей адресации.
![alt-текст]()
#### Часть 3. Проверка сетевых подключений.
##### Шаг 1. Отобразим конфигурацию коммутатора.
##### Шаг 2. Протестируем сквозное соединение, отправив эхо-запрос.
##### Шаг 3. Проверим удаленное управление коммутатором S1.
