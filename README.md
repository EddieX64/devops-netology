1. Функция `chdir("/tmp")`

2. Команда `file` использует "magic patterns", которые находятся в `/usr/share/misc/magic.mgc` или в `/usr/share/misc/magic`

3. `lsof | grep deleted` Находим pid процесса, который использует удаленный файл и опустошаем его `> /proc/*pid*/fd/*x*`

4. Зомби-процессы не потребляют никаких ресурсов, кроме записей в таблице процессов.

5. После отработки команды `opensnoop-bpfcc -T` в течение секунды, видим вызовы к следующим файлам:
```
/proc/interrupts
/proc/stat
/proc/irq/20/smp_affinity
/proc/irq/11/smp_affinity
/proc/irq/0/smp_affinity
/proc/irq/1/smp_affinity
/proc/irq/4/smp_affinity
/proc/irq/8/smp_affinity
/proc/irq/12/smp_affinity
/proc/irq/14/smp_affinity
/proc/irq/15/smp_affinity
/var/run/utmp
/usr/local/share/dbus-1/system-services
/usr/share/dbus-1/system-services
/lib/dbus-1/system-services
/var/lib/snapd/dbus-1/system-services/
/proc/574/cgroup
```

6. `uname -a` использует системный вызов функции uname(). "Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}."

7. Разница между `;` и `&&` в том, что `;` - оператор безусловного последовательного выполнения команд, в то время как при `&&` - команда не выполняется, если предыдущая выполнилась неуспешно. Смысл использовать `&&` после ввода команды `set -e` есть, т.к. поведение оператора `&&` не меняется, но если команда вернула не non-zero exit-code, то мы выходим из под текущего юзера.

8. `set -euxo pipefail`
```
-e  выполнять exit при выполнении команды с non-zero exit code
-u  раcценивать необъявленные переменные как ошибку
-x  выводить команды и аргументы по мере их выполнения
-o  дополнительная опция переменной, в нашем случае pipefail.
```
pipefail - возвращаемое значение pipe это статус последней команды, которая завершилась с non-zero exit-code или 0, если ни одна команда не завершилась с non-zero exit-code.
Другими словами, если любая команда выполняется с non-zero exit-code, то скрипт прерывается. В скриптах полезно использовать для контроля выполнения всех условий.

9. Наиболее часто встречающийся статус - S, что значит прерываемый сон процесса (процесс ожидает события)
 Дополнительные статусы означают:
 ```
 <  высокий приоритет 
 N  низкий приоритет
 L  имеет заблокированные страницы в памяти
 s  лидер сессии
 l  многопоточный процесс
 +  находится в группе foreground
 ```
