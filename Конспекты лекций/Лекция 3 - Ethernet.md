## План:
Понятие канального протокола, его задачи
Сети Ethernet
* Алгоритм работы (CSMA\CD)
* Физический уровень
* Алгоритм работы коммутатора
Аппаратные адреса
Типы кадров Ethernet
Разновидности Ethernet 
Оборудование Ethernet
Алгоритма работы L2 коммутатора

# Задачи канального протокола

<details>
  <summary>Цель.</summary>\
Управление доступом к каналу связи и обеспечение эффективной связи сетевых узлов. 
</details>

<details>
  <summary> Управление доступом к каналу.</summary>
Канал связи – общий разделяемый ресурс. Протокол предотвращает или разрешает конфликт. 
</details>

<details>
  <summary> Адресация. </summary>
 Адреса есть у каждого сетевого узла для идентификации источника и назначения сетевого трафика. MAC (Media Access Control Address). 
</details>

<details>
  <summary>Управление топологией. </summary>
Регламентация физической и управление логической топологией и фильтрацией трафика.
</details>

<details>
  <summary>  Обнаружение и исправление ошибок. </summary>
Контрольные суммы или проверки циклическим избыточным кодом (CRC). 
</details>

<details>
  <summary>Управление потоком. </summary>
  Протокол согласует скорости передачи.
</details>

#### Как разделяются каналы?
<details>
  <summary>Мультиплексирование с частотным разделением каналов (FDM).</summary>
  Деление полосы пропускания на непересекающиеся частотные диапазоны. FDM используется в аналоговых системах связи, таких как радио- и телевещание. 
</details>

<details>
  <summary>Мультиплексирование с временным разделением (TDM).</summary>
 Делит канал связи на временные интервалы, каждый из которых назначается отдельному сигналу. Каждый сигнал занимает свой собственный временной интервал, и временные интервалы чередуются для создания непрерывного потока данных. TDM обычен для компьютерных сетей.  
</details>

Подходы FDM и TDM могут комбинироваться.


#### Какие есть подходы к управлению передачей пакетов?

<details>
  <summary>Коммутация каналов (Circuit Switching). </summary>
Метод разделения канала, когда между узлами устанавливаются постоянные или существующие на момент передачи логические или физические каналы. Плюсы скорость передачи, минусы нерациональное использование возможности сети.
</details>


<details>
  <summary>Коммутация пакетов (Packet Switching).</summary>
Метод разделения канала во времени, когда задача передачи пакетов одного потока решается отдельно для каждого пакета. Плюсы – эффективное использование возможностей, потенциальная отказоустойчивость. Минусы – избыточность, нет гарантий скорости.
</details>

Эти принципиальные подходы используются и на канальном и на сетевом уровнях.

#### Что понимают под «стандартом сети»?

Когда говорят о «стандарте сети» говорят о: 
* канальном протоколе, как алгоритме, 
* спецификации физического уровня (провода, разъемы, дистанции), 
* топологиях

# Ethernet

#### IEEE 802.3
Описывает все, что связано с Ethernet 
* физический уровень, 
* кодирование, 
* формат кадров 
* протокол управления доступом к среде 
* другие служебные протоколы

#### Carrier Sense Multiple Access with Collision Detection(CSMA\\CD. )
У всех протоколов Ethernet один и тот же протокол доступа к каналу.

Carrier Sense Multiple Access with Collision Detection (множественный доступ с прослушиванием несущей и обнаружением коллизий) 
* Равноправный доступ 
* Отсутствие диспетчеризации 
* Статистические механизмы

#### Прием сигнала в CSMA\\CD
1. Обнаружение сигналов в линии - если есть сигнал на несущей частоте, переходит на 2 этап
2. Установить синхронизацию и ожидание начального ограничителя - начинает принимать сигнал
3. Ограничитель обнаружен - если обнаружен ___data trailer___, то переходим на этап 4. Если не обнаружен, то ждем сигналы и переходим на этап 1.
5. Адреса совпадают - если MAC адреса в заголовке совпадают с MAC адресом узла, то принимаем сигнал. Иначе ждем сигнал и переходим в 1.

![](/images/Прием_CSMA_CD.png)

#### Передача в CSMA\\CD
Interpacket gap (IPG) или Interframe gap (IFG) – это минимальный промежуток времени между передаваемыми пакетами (кадрами); 
* 9,6 мкс для Ethernet (10 Мб/c) 
* 0,96 мкс для Fast Ethernet (100 Мб/c) 
* 2,4 нс для 40 GigabitEthernet (40 Гб/с)
* и т.д.


![](/images/Передача_CSMA_CD.png)

###### Как выбирается время задержки?
$P = L \cdot T$ 
$Т=512$ битовых интервалов, так как это минимальный размер кадра.
$L$ - случайное число из $[0, 2^N]$, которое при $N \geq 10$ равно 1023, где $N$ - номер последовательно возникшей коллизии. 


#### Домен коллизий
___Домен коллизий___ – это область локальной сети, где узлы могут вступать в коллизию, передавая данные одновременно.
> Не следует путать с ___широковещательным доменом.___
> ___Широковещательный домен___ - это область сети, в которой все устройства могут получать широковещательные сообщения, например, если есть маршрутизатор, то все устройства, подключенные к нему, образуют один широковещательный домен
> ___Широковещательный домен___ - это сущность ___канального уровня___, а домен коллизий - это сущность физического уровня

# MAC адреса
MAC адрес состоит из 2 частей: OUI и VID
 Дальше про каждую часть. Про общий консорциум, который выдает OUI адреса(ограничение на набор MAC адресов)
 Первый байт OUI содержит 2 бита, которые не являются случайными.
 7 бит в OUI является индикатором того, является ли MAC выставленным производителем(0) или назначенным администратором(1). Это делается для того, чтобы скрыть производителя оборудования, так как иначе по заголовкам в пакетах можно определить OUI из MAC адреса