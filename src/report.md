# D01_Linux
Here you can see completed project tasks

## Part 1. Installation of the OS
* Команда для просмотра версии:  `cat /etc/issue`

* **etc** - в этой директории находятся файлы с найстройками системы и многих программ<br>
![linux version](../misc/images_for_report/linux_version.png)

## Part 2. Creating a user
* `useradd` - это утилита командной строки, которую можно использовать для создания новых пользователей в системах Linux и Unix. Общий синтаксис команды следующий: `useradd [OPTIONS] USERNAME`

> Только root или пользователи с привилегиями sudo могут создавать новые учетные записи пользователей с помощью useradd.

* Для добавления в группы пользователя используется флаг `-G`

* Использованная команда: `sudo useradd -G adm tempUser` <br>
![creating user command](../misc/images_for_report/create_user1.png)

* Вывод команды `cat /etc/passwd`: <br>
![output passwd](../misc/images_for_report/create_user2.png)

## Part 3. Setting up the OS network
* ___Задание названия машины вида user-1___ <br>
`hostname` - позволяет проверить текущее название имя хоста комрьютера <br>
`hostnamectl` - отображает дополнительную информацию о вашей компьютерной системе <br>
`hostnamectl set-hostname new-hostname` - изменяет имя хоста машины <br>
![machine name](../misc/images_for_report/change_hostname.png)

* ___Установка временной зоны, соответствующей моему текущему местоположению.___ <br>
Утилита `timedatectl` применяется для настройки и получения информации о текущем системном времени. Она доступна в системах, использующих systemd. <br>
![timezone](../misc/images_for_report/timezone.png) <br>
Для установки часового пояса с помощью утилиты `timedatectl` нужно выполнить команду: `timedatectl set-timezone Europe/Moscow` <br>
С помощью команды `timedatectl list-timezones` можно увидеть список всех доступных временных зон.

* ___Вывод названия сетевых интерфейсов с помощью консольной команды.___ <br>
Существует множество способов вывода названий сетевых интерфейсов, например: `ifconfig`(depricated), `netstat -i`, `ip address` и др. <br>
![network interfaces](../misc/images_for_report/network_interfaces.png) <br>
> lo (loopback device) – виртуальный интерфейс, присутствующий по умолчанию в любом Linux. Он используется для отладки сетевых программ и запуска серверных приложений на локальной машине. С этим интерфейсом всегда связан адрес 127.0.0.1. У него есть dns-имя – localhost. Посмотреть привязку можно в файле /etc/hosts.

* ___Используя консольную команду, получи ip адрес устройства, на котором ты работаешь, от DHCP сервера.___ <br>
Примером DHCP-сервера является LAN-роутер. <br>
Для получение ip адреса устройства можно использовать команды: `ip address`, `hostname -I` и др. <br>
![ip address from DHCP](../misc/images_for_report/ip_address_from_DHCP.png)
> DHCP — протокол прикладного уровня модели TCP/IP, служит для назначения IP-адреса клиенту. Это следует из его названия — Dynamic Host Configuration Protocol. <br>

* ___Определи и выведи на экран внешний ip-адрес шлюза (ip) и внутренний IP-адрес шлюза, он же ip-адрес по умолчанию (gw).___ <br>
![gateway ip-adress](../misc/images_for_report/gateway_ip_address.png)

* ___Задай статичные (заданные вручную, а не полученные от DHCP сервера) настройки ip, gw, dns (используй публичный DNS серверы, например 1.1.1.1 или 8.8.8.8).___ <br>
Конфигурационный файл для изменения данных: <br>
![config data](../misc/images_for_report/config_file_for_change_data.png)
> Для работы с изменением данных понадобилось установить NetworkManager.
> Для изменения применялась команда: `sudo netplan apply`, после требуется обновить, можно использовать команду: `sudo systemctl restart NetworkManager` <br>

* ___Перезагрузи виртуальную машину. Убедись, что статичные сетевые настройки (ip, gw, dns) соответствуют заданным в предыдущем пункте.___ <br>
Теперь мы видим новый ip-адрес: <br>
![new ip address](../misc/images_for_report/new_ip_address.png) <br>
Пингуем ya.ru и 1.1.1.1: <br>
![ping yandex](../misc/images_for_report/ping_ya.png)
![ping 1.1.1.1](../misc/images_for_report/ping1.1.png)

## Part 4. OS Update
* Сущесвует две важные команды, связанные с обновлением пакетов:
`sudo apt update` - обновляет информацию об актуальных версиях доступных пакетов<br>
`sudo apt upgrade` - обновляет пакеты <br>
![upgrade packages](../misc/images_for_report/upgrade.png)

## Part 5. Using the sudo command
> sudo (англ. Substitute User and do, дословно «подменить пользователя и выполнить») — программа для системного администрирования UNIX-систем, позволяющая делегировать те или иные привилегированные ресурсы пользователям с ведением протокола работы. Основная идея — дать пользователям как можно меньше прав, при этом достаточных для решения поставленных задач. Программа поставляется для большинства UNIX и UNIX-подобных операционных систем. <br>
* Чтобы пользователю дать права sudo, его нужно добавить в группу sudo: `usermod -a -G sudo [user]`
* Чтобы сменить пользоваетеля, можно использовать команду `sudo -i -u [user]`, где -i(login) и -u(user) <br>
![change hostname](../misc/images_for_report/change_hostname_other_user.png)

## Part 6. Installing and configuring the time service
* Для переключение на более надежный проткол NTPD (Network Time Protocol daemon) выполняются следующий шаги:
    1. Отключается стандартная утилита, команда: `sudo timedatectl set-ntp no`
    2. Обновить пакеты системы, команда: `sudo apt update`
    3. Установить ntp: `sudo apt install ntp` <br>
    Вывод команды `timedatectl show`: <br>
    ![time management](../misc/images_for_report/timemanagment.png)

## Part 7. Installing and using text editors

* Используя каждый из трех выбранных редакторов, создай файл test_X.txt, где X -- название редактора, в котором создан файл. Напиши в нём свой никнейм, закрой файл с сохранением изменений. <br>
    1. Vim: (для выхода и сохранения: esc -> shift+: -> wq) <br>
    ![vim1](../misc/images_for_report/vim1.png) <br>
    2. Nano: (для выхода: ctrl+o сохранение, ctrl+x выход) <br>
    ![nano1](../misc/images_for_report/nano1.png) <br>
    3. Joe: (для сохранения и выхода: ctrl+k+x) <br>
    ![joe1](../misc/images_for_report/joe1.png) <br>

* Используя каждый из трех выбранных редакторов, открой файл на редактирование, отредактируй файл, заменив никнейм на строку «21 School 21», закрой файл без сохранения изменений. <br>
    1. Vim: (сначала в режим редактирования с помощью i, для выхода и сохранения: esc -> shift+: -> q!, чтобы не сохранять изменения) <br>
    ![vim2](../misc/images_for_report/vim2.png)
    2. Nano: (ctrl+x, далее выбрать no) <br>
    ![nano2](../misc/images_for_report/nano2.png)
    3. Joe: (ctrl+c, далее выбрать yes) <br>
    ![joe2](../misc/images_for_report/joe2.png)

* Используя каждый из трех выбранных редакторов, отредактируй файл ещё раз (по аналогии с предыдущим пунктом), а затем освой функции поиска по содержимому файла (слово) и замены слова на любое другое. <br>
    1. Vim: заходим в режим поиска через esc, пришем через / слово, редактируем поимвольно через r <br>
    ![alt text](../misc/images_for_report/vim31.png)
    ![alt text](../misc/images_for_report/vim32.png)
    2. Nano: замена с помощью команды ctrl+\ <br>
    ![alt text](../misc/images_for_report/nano3.png)
    3. Joe: ctrl+f, ищем слово, следуем указаниям редактора <br>
    ![alt text](../misc/images_for_report/joe3.png)

## Part 8. Installing and basic setup of the SSHD service
* Устанавливаем ssh
* Командой `systemctl status sshd` проверяем работоспособность: <br>
![alt text](../misc/images_for_report/check_sshd.png)
* Командой `systemctl list-unit-files --type=service --state=enabled` можно проверить, какие команды автоматически загружаются при загрузке системы. Также можно проверять отдельные службы с помощью: `sudo systemctl is-enabled [service]`
* С помощью `sudo systemctl status [service]` можно проверить, находится ли сервер в автозагрузке и запущен ли он сейчас
![alt text](../misc/images_for_report/check_sshd_state.png)
* Смена порта:
    1. Открытие конфигурационного файла: `sudo vim /etc/ssh/sshd_config`
    2. Редактирование порта <br>
    ![alt text](../misc/images_for_report/edit_port.png)
    3. Перезапуск SSH-сервера: `sudo systemctl restart sshd` <br>
    ![alt text](../misc/images_for_report/modified_port_status.png)
* Используя команду ps, покажи наличие процесса sshd. Для этого к команде нужно подобрать ключи. <br>
Используем команду: `ps -A -f | grep "ssh"` <br>
-f - вывести максимум доступных данных, например, количество потоков<br>
-A - выбрать все процессы <br>
* Вывод `netstat -tan`: <br>
![alt text](../misc/images_for_report/netstat_output.png) <br>
__-t__ - выдает TCP-порты <br> 
__-a__ - отображать все сокеты <br>
__-n__ - разрешать имена <br>
__proto__ - протокол, используемый сокетом <br>
__Recv-Q__ - счётчик байт не скопированных программой пользователя из этого сокета <br>
__Send-Q__ - счётчик байтов, не подтверждённых удалённым узлом <br>
__Local Address__ - адрес и номер порта локального конца сокета. Если не указана опция --numeric (-n), адрес сокета преобразуется в каноническое имя узла (FQDN), и номер порта преобразуется в соответствующее имя службы <br>
__Foreign Address__ - адрес и номер порта удалённого конца сокета. Аналогично "Local Address." <br>
__State__ - состояние сокета <br>
0.0.0.0 является немаршрутизируемым мета-адресом, используемым для обозначения недопустимой, неизвестной или не применимой цели (нет конкретного адресатора). В контексте серверов 0.0.0.0 означает «все адреса IPv4 на локальном компьютере»

## Part 9. Installing and using the top, htop utilities
Команда top показывает запущенные в Linux процессы программ и служб, данные о потреблении системных ресурсов и позволяет искать, останавливать процессы и управлять ими. <br>
* В первой строке последовательно: uptime, количество авторизованных пользователей, общая загрузка системы
* Во второй строке: общее колчиество процессов
* Третья строка: загрузка CPU
* Четвертая строка: загрузка памяти
* pid процесса, занимающего больше всего памяти: 505 (с помощью x - сортировка)
* pid процесса, занимающего больше всего процессорного времени: 505 (с помощью x - сортировка) <br>
![alt text](../misc/images_for_report/top.png)

Команда htop выполняет примерно ту же задачу, что и top, но имеет определённые преимущества и недостатки: более удобные поиск и фильтрация, но менее гибкая настройка отображения процессов. <br>
* Сортировка по определенному параметру производится с помощью sortBy: <br>
![alt text](../misc/images_for_report/sort_htop.png) <br>
* Фильтрация с помощью filter: <br>
![alt text](../misc/images_for_report/filter_htop.png) <br>
* Поиск с помощью search: <br>
![alt text](../misc/images_for_report/search_htop.png) <br>
* Добавление в вывод дополнительной информации: <br>
![alt text](../misc/images_for_report/add_some_info_htop.png) <br>

## Part 10. Using the fdisk utility

* __fdisk__ - это команда для управления разделами жёсткого диска, а также получения информации о них. <br>
![alt text](../misc/images_for_report/fdisk.png) <br>
Измерения производятся в МиБ (мебибайт) <br>
* Для нахождения размера swap: `free -h` <br>
![alt text](../misc/images_for_report/swap.png) <br>

## Part 11. Using the df utility
* __df__ - это команда для получения подробного отчета об использовании дискового пространства системы. <br>
* ![alt text](../misc/images_for_report/df_slash.png) <br>
    1. Размер раздела: 1055762868
    2. Размер занятого пространства: 2177992
    3. Размер свободного пространства: 999881404 
    4. Процент использования: 1% <br>
    Размер выводится в байтах <br>

* ![alt text](../misc/images_for_report/df2.png) <br>
    1. Размер раздела: 1007G 
    2. Размер занятого пространства: 2.1G 
    3. Размер свободного пространства: 954G  
    4. Процент использования: 1% <br>
    Тип файловой системы: ext4 <br>
    __Ext4__ - ext4 (англ. fourth extended file system, ext4fs) — журналируемая файловая система, используемая преимущественно в операционных системах с ядром Linux, созданная на базе ext3 в 2006 году. <br>

## Part 12. Using the du utility
* __du__ - это команда для получения приблизительного объема дискового пространства, используемого указанными при вызове команды файлами или каталогами. <br>
* Чтобы выводить информацию в человеко читаемом виде: `du -sh` <br>
    __-s__ - ключ вывода только общего размера <br>
    __-h__ - вывод в более читабельном виде <br>
* /home <br>
![alt text](../misc/images_for_report/duHome.png) <br>
* /var <br>
![alt text](../misc/images_for_report/duVar.png) <br>
* /var/log <br>
![alt text](../misc/images_for_report/duVarLog.png) <br>
* /var/log/* <br>
![alt text](../misc/images_for_report/duVarLogAll.png) <br>

## Part 13. Installing and using the ncdu utility
* __ncdu__ - это команда, имеющая то же назначение, что и du, но обладающая приятным и удобным интерфейсом. <br>
* /home <br>
![alt text](../misc/images_for_report/ncduHome.png) <br>
* /var <br>
![alt text](../misc/images_for_report/ncduVar.png) <br>
* /var/log <br>
![alt text](../misc/images_for_report/ncduVarLog.png) <br>

## Part 14. Working with system logs
* Время последней успешной авторизации: Jul 20 21:36:04 for user root(uid=0) by yura(uid=1000)
* Перезапуск службы SSHd: `sudo sysyemctl restart ssh`
![alt text](../misc/images_for_report/log.png)

## Part 15. Using the CRON job scheduler
* CRON - это программа-демон. Её основная задача выполнять указанные пользователем процессы в указанное пользователем время, например с определённой периодичностью. <br>
* Запуск uptime через каждые две минуты:
    1. Открытие: `crontab -e`, добавление строки: */2 * * * * uptime
    2. Поиск в логах: `cat /var/log/syslog` <br>
    ![alt text](../misc/images_for_report/cronuptimelog.png)
    3. Список текущих задач: `crontab -l` <br>
    ![alt text](../misc/images_for_report/crontab.png)
    4. Удаление всех заданий из планировщика: `crontab -r` <br>
    ![alt text](../misc/images_for_report/crontabdel.png)
