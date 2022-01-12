1. В Windows для этого есть команда `ipconfig /all`. В Linux можно использовать `ifconfig` или `ip a`

2. Протокол LLDP, пакет lldpd, команда `lldpctl`

3. VLAN. в Linux есть пакет vlan. Можно использовать `vconfig`, но после перезагрузки интерфейс будет удален. Лучше добавить напрямую в `/etc/network/interfaces`
   Пример конфига:
   ```
   ##vlan с ID-100 для интерфейса eth0 with ID - 100 в Debian/Ubuntu Linux##
   auto eth0.100
   iface eth0.100 inet static
   address 192.168.1.200
   netmask 255.255.255.0
   vlan-raw-device eth0
   ```

4. Типы агрегации:

	Mode-0(balance-rr);
	Mode-1(active-backup);
	Mode-2(balance-xor);
	Mode-3(broadcast);
	Mode-4(802.3ad);
	Mode-5(balance-tlb);
	Mode-6(balance-alb).
	
	Опции:

	downdelay - время ожидания в миллисекундах перед отключением устройства после обнаружения сбоя связи;
	miimon - частота мониторинга канала в миллисекундах;
	updelay - время ожидания в миллисекундах перед включением устройства после восстановления связи.
	
	Пример конфига:
	```	
	vagrant@vagrant:~$ sudo nano /etc/network/interfaces
	# interfaces(5) file used by ifup(8) and ifdown(8)
	# Include files from /etc/network/interfaces.d:
	source-directory /etc/network/interfaces.d
	##vlan с ID-100 для интерфейса eth0 with ID - 100
	auto eth0.100
	iface eth0.100 inet static
	address 192.168.1.200
	netmask 255.255.255.0
	vlan-raw-device eth0

	auto eth0.200
	iface eth0.200 inet static
	address 192.168.1.250
	netmask 255.255.255.0
	vlan-raw-device eth0

	# The bond0 network interface
	auto bond0
	allow-hotplug bond0
	iface bond0 inet static
	address 192.168.1.150
	netmask 255.255.255.0
	gateway 192.168.1.1
	dns-nameservers  192.168.1.1 8.8.8.8
	dns-search domain.local
			slaves eth0.100 eth0.200
			bond_mode 4
	```

5. 8 адресов всего, адрес подсети и broadcast зарезервированы, 6 доступно под хосты. 10.10.10.0/24 можно разделить на 32 подсети 10.10.10.0/29.
   Примеры: 10.10.10.0/29; 10.10.10.8/29 и т.д.

6. Возьмем диапазон 100.64.0.0 — 100.127.255.255 с префиксом /10 (255.192.0.0)
   ```
   ipcalc 100.64.0.0/10 -s 50
   Needed size:  64 addresses.
   Used network: 100.64.0.0/26
   ```

7. В Windows `arp -a`, для очистки `netsh interface ip delete arpcache`. 
   В Linux утилита arp из пакета net-tools, удалить один адрес `arp -d`, для полной очистки `ip neigh flush all`
