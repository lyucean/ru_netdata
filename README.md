# System Overview

# Users
Статистика по пользователям собирается с помощью плагина `netdata` `apps.plugin`. Этот плагин 
просматривает все процессы и собирает статистику по каждому пользователю. 

Отчёт по значениям совпадает с теми, что выводить команда `top`, хотя плагин `netdata` также считает 
ресурсы дочерних процессов (в отличие от `top`, который показывает только ресурсы текущих запущенных 
процессов). Таким образом, для процессов `shell scripts`, график так же содержит затрачиваемые ресурсы, 
используемые командами, которые эти сценарии запускают в каждом временном интервале.

## cpu
>Если установить через docker, есть проблема т.к. имя пользователя, `netdata` получат `getpwuid()` 
>(а она видет только локальных пользоватлей), то вместо имён будут `uid`. [Решение проблемы](https://github.com/netdata/netdata/pull/6472)
> - Как узнать uid пользователя  по имени `id -u mysql` 
> - Как узнать имя пользователя по uid `id -nu 103`

Графики в разделе:
- Время ЦП пользователей (1600% = 16 ядер) (users.cpu)
- Пользовательское время ЦП пользователей (1600% = 16 ядер) (users.cpu_user)
- Системное время процессора пользователей (1600% = 16 ядер) (users.cpu_system)


# CPUs
Подробная информация для каждого процессора системы. Краткое описание системы для всех процессоров 
можно найти в разделе [«Обзор системы»](#System Overview).

Графики в разделе:
- Utilization - процент нагрузки на каждое ядро, разбит по процессам
- Interrupts - прерывания
- - NMI (немаскируемые прерываний) - обрабатываются всегда, независимо от запретов на другие прерывания. 
К примеру, такое прерывание может быть вызвано сбоем в микросхеме памяти.
    
- - RES (перепланирование прерываний) - это способ Linux kernel разбудить простаивающее CPU-ядро, 
    чтобы запланировать поток на нем. 
>Планировщик пытается распределить активность процессора между как можно большим количеством ядер. 
>Общее эмпирическое правило состоит в том, что предпочтительнее иметь как можно больше процессов, 
>работающих на всех ядрах с более низкой мощностью (более низкие тактовые частоты), а не иметь 
>одно ядро, действительно занятое работой на полной скорости, в то время как другие ядра спят.
    
- - LOC (локальные прерывания таймера) - это таймер, реализованный на APIC, который прерывает только 
определенный CPU вместо того, чтобы вызывать прерывание, которое может быть обработано любым CPU.
>Например, проверка, не нужно ли нам переключиться на другой процесс. это LOC прерывание
    
- - CAL (прерывания вызова функций) - прерывания на выходе `/proc/interrupts`, может со временем уменьшаться.
В некоторых случаях он может вести себя странно - отображать очень большие значения или быть нестабильным.
    
- - TLB (таблица страниц) - [wiki](https://ru.wikipedia.org/wiki/%D0%A2%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0_%D1%81%D1%82%D1%80%D0%B0%D0%BD%D0%B8%D1%86)
- - MCP (Опросы для проверки машин)
    
- Softirq - отложенные прерывания
>Отложенная обработка прерывания предполагает, что некоторая часть действий по обработке 
>результатов прерывания может быть отложена на более позднее время, когда ядро системы будет 
>менее загружено или будет находиться в более стабильном состоянии.
- - TIMER (Обработчик прерываний таймера)
- - NET_TX (Отправка сетевых пакетов)
- - NET_RX (Прием сетевых пакетов)
- - TASKLET
>Тасклеты — это механизм обработки нижних половин, построенный на основе механизма отложенных прерываний. 
>Как уже отмечалось, они не имеют ничего общего с заданиями (task). Тасклеты по своей природе и 
>принципу работы очень похожи на отложенные прерывания. Тем не менее они имеют более простой 
>интерфейс и упрощенные правила блокировок.
- - SCHED (выбирает  для  выполнения  следующий процесс)
- - RCU (Read-Copy-Update, RCU (Чтение-модификация-запись) [wiki](https://ru.wikipedia.org/wiki/Read-copy-update)
>Механизм синхронизации в многопоточных системах. Реализует неблокирующую синхронизацию для 
>всех читателей структуры данных. Запись может производиться параллельно чтению, однако 
>одновременно может быть активен только один писатель.

- softnet
