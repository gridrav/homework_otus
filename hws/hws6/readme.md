#  Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями

###  Топология:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/top1.png)

###     Таблица адресации:

| Устройство       | Интерфейс      | IP-адрес                 |Маска подсети            |   Шлюз по умолчанию  |    
|:-----------------|:---------------|:-------------------------|:------------------------|:---------------------|
| R1               | G0/0/1.10      |192.168.10.1              |  255.255.255.0          |  -                   |       
|                  | G0/0/1.20      |192.168.20.1              |  255.255.255.0          |  -                   |  
|                  | G0/0/1.30      |192.168.30.1              |  255.255.255.0          |  -                   |  
|                  | G0/0/1.1000    |-                         |-                        |                      |
| S1               | VLAN 10        | 192.168.10.11            |  255.255.255.0          | 192.168.10.1         |
| S2               | VLAN 10        | 192.168.10.12            |  255.255.255.0          | 192.168.10.1         | 
| PC-A             | NIC            | 192.168.20.3              | 255.255.255.0          | 192.168.20.1         | 
| PC-B             | NIC            | 192.168.30.3              | 255.255.255.0          | 192.168.30.1         | 

### Таблица VLAN:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/top2.png)

### Задачи

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Создание сетей VLAN и назначение портов коммутатора

Часть 3. Настройка транка 802.1Q между коммутаторами.

Часть 4. Настройка маршрутизации между сетями VLAN

Часть 5. Проверка, что маршрутизация между VLAN работает

Общие сведения/сценарий
       
В целях повышения производительности сети большие широковещательные домены 2-го уровня делят на домены меньшего размера. Для этого современные коммутаторы используют виртуальные локальные сети (VLAN). VLAN также можно использовать в качестве меры безопасности, отделяя конфиденциальный трафик данных от остальной части сети. Сети VLAN облегчают процесс проектирования сети, обеспечивающей помощь в достижении целей организации. Для связи между VLAN требуется устройство, работающее на уровне 3 модели OSI. Добавление маршрутизации между VLAN позволяет организации разделять широковещательные домены, одновременно позволяя им обмениваться данными друг с другом.

Транковые каналы сети VLAN используются для распространения сетей VLAN по различным устройствам. Транковые каналы разрешают передачу трафика из множества сетей VLAN через один канал, не нанося вред идентификации и сегментации сети VLAN. Особый вид маршрутизации между VLAN, называемый «Router-on-a-Stick», использует магистраль от маршрутизатора к коммутатору, чтобы все VLAN могли переходить к маршрутизатору.

В этой лабораторной работе создадите VLAN на обоих коммутаторах в топологии, назначите VLAN для коммутации портов доступа, убедитесь, что VLAN работают должным образом, создадите транки VLAN между двумя коммутаторами и между S1 и R1, и настройте маршрутизацию между VLAN на R1 для разрешения связи между хостами в разных VLAN независимо от подсети, в которой находится хост.

###  Инструкции

#### Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

##### Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/top3.png)

##### Шаг 2. Настройте базовые параметры для маршрутизатора.

+ Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.
  
Откройте окно конфигурации

+ Войдите в режим конфигурации.
+ Назначьте маршрутизатору имя устройства.
+ Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
+ Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
+ Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
+ Установите cisco в качестве пароля виртуального терминала и активируйте вход.
+ Зашифруйте открытые пароли.
+ Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

  ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/r1.png)
  
+ Сохраните текущую конфигурацию в файл загрузочной конфигурации.
+ Настройте на маршрутизаторе время.
Закройте окно настройки.

 ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/r1_1.png)

##### Шаг 3. Настройте базовые параметры каждого коммутатора.

+ Присвойте коммутатору имя устройства.
+ Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
+ Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
+ Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
+ Установите cisco в качестве пароля виртуального терминала и активируйте вход.
+ Зашифруйте открытые пароли.
+ Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
+ Настройте на коммутаторах время.
+ Сохранение текущей конфигурации в качестве начальной.
Закройте окно настройки.

Для первого коммутатора

 ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_1.png)

 ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_2.png)

Для второго коммутатора

 ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_3.png)
 
 ![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_4.png)

##### Шаг 4. Настройте узлы ПК.

Адреса ПК можно посмотреть в таблице адресации.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_5.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_6.png)
        
#### Часть 2. Создание сетей VLAN и назначение портов коммутатора

Во второй части вы создадите VLAN, как указано в таблице выше, на обоих коммутаторах. Затем вы назначите VLAN соответствующему интерфейсу и проверите настройки конфигурации. Выполните следующие задачи на каждом коммутаторе.

##### Шаг 1. Создайте сети VLAN на коммутаторах.

+ Создайте и назовите необходимые VLAN на каждом коммутаторе из таблицы выше.
Откройте окно конфигурации

```
S1#conf t
S1(config)#vlan 10
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 20
S1(config-vlan)#name Sales
S1(config-vlan)#exit
S1(config)#vlan 30
S1(config-vlan)#name Operations
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name Parking_lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#
```

```
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#vlan 10
S2(config-vlan)#name Management
S2(config-vlan)#exit
S2(config)#vlan 20
S2(config-vlan)#name Sales
S2(config-vlan)#exit
S2(config)#vlan 30
S2(config-vlan)#name Operations
S2(config-vlan)#exit
S2(config)#vlan 999
S2(config-vlan)#name Parking_Lot
S2(config-vlan)#exit
S2(config)#vlan 1000
S2(config-vlan)#name Native
S2(config-vlan)#exit
S2(config)#
```

+ Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации.
  
```
S1(config)#
S1(config)#interface vlan 10
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

S1(config-if)#ip address 192.168.10.11 255.255.255.0
S1(config-if)#no shut
S1(config-if)#no shutdown 
S1(config-if)#exit
S1(config)#ip def
S1(config)#ip default-gateway 192.168.10.1
S1(config)#
```

Для S2:

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_7.png)

+ Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их.
Примечание. Команда interface range полезна для выполнения этой задачи с минимальным количеством команд.

```
S1(config)#
S1(config)#interface range f0/2-4, f0/7-24, g0/1-2
S1(config-if-range)#swi
S1(config-if-range)#switchport mode ac
S1(config-if-range)#switchport mode access 
S1(config-if-range)#sw
S1(config-if-range)#switchport ac
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#description unused_parking
S1(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
S1(config-if-range)#exit
S1(config)#
```

```
S2(config)#
S2(config)#interface range f0/2-17, f0/19-24, g0/1-2
S2(config-if-range)#swi
S2(config-if-range)#switchport mode ac
S2(config-if-range)#switchport mode access 
S2(config-if-range)#sw
S2(config-if-range)#switchport ac
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#de
S2(config-if-range)#description unused_parking
S2(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/5, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
S2(config-if-range)#exit
S2(config)#
```

##### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

+ Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_8.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_9.png)

+ Убедитесь, что VLAN назначены на правильные интерфейсы.
Закройте окно настройки.

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_10.png)

![alt-текст](https://github.com/gridrav/homework_otus/blob/main/hws/hws6/Screens/Screenshot_11.png)

#### Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

В части 3 вы вручную настроите интерфейс F0/1 как транк.

##### Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.

+ Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.
Откройте окно конфигурации
+ Установите native VLAN 1000 на обоих коммутаторах.
+ Укажите, что VLAN 10, 20, 30 и 1000 могут проходить по транку.
+ Проверьте транки, native VLAN и разрешенные VLAN через транк.

##### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

+ Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.
+ Сохраните текущую конфигурацию в файл загрузочной конфигурации.
+ Проверка транкинга.

Вопрос:
Что произойдет, если G0/0/1 на R1 будет отключен?
Закройте окно настройки.

#### Часть 4. Настройка маршрутизации между сетями VLAN

##### Шаг 1. Настройте маршрутизатор.

Откройте окно конфигурации
+ При необходимости активируйте интерфейс G0/0/1 на маршрутизаторе.
+ Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.
+ Убедитесь, что вспомогательные интерфейсы работают
Закройте окно настройки.

#### Часть 5. Проверьте, работает ли маршрутизация между VLAN

##### Шаг 1. Выполните следующие тесты с PC-A. Все должно быть успешно.

Примечание. Возможно, вам придется отключить брандмауэр ПК для работы ping
+ Отправьте эхо-запрос с PC-A на шлюз по умолчанию.
+ Отправьте эхо-запрос с PC-A на PC-B.
+ Отправьте команду ping с компьютера PC-A на коммутатор S2.

##### Шаг 2. Пройдите следующий тест с PC-B

В окне командной строки на PC-B выполните команду tracert на адрес PC-A.
Вопрос:
Какие промежуточные IP-адреса отображаются в результатах?
