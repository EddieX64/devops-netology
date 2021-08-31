1. Предварительно создаем директорию и файл `/etc/sysconfig/node_exporter` и добавляем в него переменную OPTIONS для запуска с опциями при необходимости
```
[Unit]
Description=Node Exporter

[Service]
EnvironmentFile=-/etc/sysconfig/node_exporter
ExecStart=/usr/local/bin/node_exporter $OPTIONS

[Install]
WantedBy=multi-user.target
```
systemctl enable node_exporter
systemctl start node_exporter

2. Все метрики, начинающиеся на `node_cpu, process_cpu, node_memory, node_network, node_disk`

3. В веб-интерфейсе Netdata видим различные метрики, которые собирает утилита, такие как `CPU, Memory, Disk, Network, Interfaces, Power supply` и т.д.

4. `dmesg -T | grep -i virtual` видим, что DMI = Virtualbox, а также systemd[1] detected virtualization oracle. Соответсвенно при помощи `dmesg` можно понять, запущена ли ОС на системе виртуализации.

5. `ulimit` используется для ресурсов, занятых процессом запуска оболочки, и может использоваться для установки системных ограничений. 
`ulimit -a` по умолчанию выставлено значение open files (fs.nr_open) = 1024 в мягком лимите. `ulimit -aH` по умолчанию выставлено значение open files (fs.nr_open) = 1048576 в жестком лимите. Максимальное значение не может превышать жесткого лимита.

6. Вводим команду `screen`, изолируем текущую сессию `unsahre -f --pid --mount-proc sleep 1h` . Открываем еще одну сессию терминала, `ps aux | grep sleep` видим из хостовой системы pid 5147 процесса `sleep 1h`, ответвленного от `unshare`, вводим `nsenter --target 5147 --pid --mount` и попадаем обратно в изолированную сессию с процессом `sleep 1h`, который имеет pid 1, проверяем `ps aux`

7. Данная команда определяет функцию `:`, которая создаст еще два форка себя бесконечно. Автоматической стабилизации помог механизм контроллера количества процессов (Process Number Contorller), который используется, чтобы позволить иерархии `cgroup` останавливать любые новые процессы из `fork () 'd` или `clone ()' d` после достижения определенного предела.
```
mkdir -p /sys/fs/cgroup/pids/parent/child
echo 2 > /sys/fs/cgroup/pids/parent/pids.max
echo $$ > /sys/fs/cgroup/pids/parent/cgroup.procs
cat /sys/fs/cgroup/pids/parent/pids.current
```
Данные команды позволят создать иерархию `cgroup`, установить лимиты и прикрепить процессы
