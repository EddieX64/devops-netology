5. По умолчанию выделено 1024 Мб RAM и 2 CPU

6. ```
   config.vm.provider "virtualbox" do |v|
	  v.memory = 2048
	  v.cpus = 2 
	  ```
	  
7. HISTFILESIZE, в мануале указывается на 703 строке

8. ignoreboth — использовать опции ‘ignorespace’ и ‘ignoredups’
ignorespace — не сохранять строки начинающиеся с символа <пробел>
ignoredups — не сохранять строки, совпадающие с последней выполненной командой

9. 225 строка, список просто выполняется в среде текущего командного интерпретатора. Эту команду называют командой группировки.

10. `touch test{1..100000}`
Создать 300000 не получилось, т.к. превышается максимальная длина аргумента командой строки (ARG_MAX)

11. конструкция [[ -d /tmp ]] проверяет наличие директории /tmp

12. ```mkdir /tmp/new_path_directory/
cp /bin/bash /tmp/new_path_directory
PATH=/tmp/new_path_directory:$PATH
type -a bash
```
13. at используется для назначения выполнения разового задания в определенной время.
batch используется для выполнения разового задания, когда средняя загрузка опускается ниже 0,8.




