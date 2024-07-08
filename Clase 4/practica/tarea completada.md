

#### Paso 1: Verificar el Estado del Sistema de Archivos y Dispositivos
- Lista los dispositivos de bloque y asegúrate de que todos los discos necesarios están disponibles y no presentan problemas.

```bash
ubuntu@testdevo:~$ ls -l /dev | grep "^b"

brw-rw---- 1 root disk      7,   0 Jul  8 13:04 loop0
brw-rw---- 1 root disk      7,   1 Jul  8 13:04 loop1
brw-rw---- 1 root disk      7,   2 Jul  8 13:04 loop2
brw-rw---- 1 root disk      7,   3 Jul  8 13:04 loop3
brw-rw---- 1 root disk      7,   4 Jul  8 13:04 loop4
brw-rw---- 1 root disk      7,   5 Jul  8 13:04 loop5
brw-rw---- 1 root disk      7,   6 Jul  8 13:04 loop6
brw-rw---- 1 root disk      7,   7 Jul  8 13:04 loop7
brw-rw---- 1 root disk      8,   0 Jul  8 13:04 sda
brw-rw---- 1 root disk      8,   1 Jul  8 13:04 sda1
brw-rw---- 1 root disk      8,  15 Jul  8 13:04 sda15
brw-rw---- 1 root disk    259,   0 Jul  8 13:04 sda16
brw-rw---- 1 root disk    253,   0 Jul  8 13:04 vda
```

- Lista los dispositivos de carácter y confirma que no hay problemas con los dispositivos de entrada/salida.

``` bash
ubuntu@testdevo:~$ ls -l /dev | grep "^c"

crw-r--r-- 1 root root     10, 235 Jul  8 13:04 autofs
crw-rw---- 1 root disk     10, 234 Jul  8 13:04 btrfs-control
crw--w---- 1 root tty       5,   1 Jul  8 13:04 console
crw------- 1 root root     10, 123 Jul  8 13:04 cpu_dma_latency
crw------- 1 root root     10, 203 Jul  8 13:04 cuse
crw------- 1 root root     10, 125 Jul  8 13:04 ecryptfs
crw-rw-rw- 1 root root      1,   7 Jul  8 13:04 full
crw-rw-rw- 1 root root     10, 229 Jul  8 13:04 fuse
crw------- 1 root root     10, 183 Jul  8 13:04 hwrng
crw-r--r-- 1 root root      1,  11 Jul  8 13:04 kmsg
crw-rw---- 1 root disk     10, 237 Jul  8 13:04 loop-control
crw-r----- 1 root kmem      1,   1 Jul  8 13:04 mem
crw-rw-rw- 1 root root      1,   3 Jul  8 13:04 null
crw-r----- 1 root kmem      1,   4 Jul  8 13:04 port
crw------- 1 root root    108,   0 Jul  8 13:04 ppp
crw------- 1 root root     10,   1 Jul  8 13:04 psaux
crw-rw-rw- 1 root tty       5,   2 Jul  8 13:11 ptmx
```

- Lista todas las particiones y asegúrate de que no hay particiones llenas o mal configuradas.

```bash
ubuntu@testdevo:~$ lsblk

NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda       8:0    0    5G  0 disk
├─sda1    8:1    0    4G  0 part /
├─sda15   8:15   0   99M  0 part /boot/efi
└─sda16 259:0    0  923M  0 part /boot
vda     253:0    0   52K  1 disk
```

- Revisa la información de montajes para asegurarte de que todas las particiones necesarias están montadas correctamente.

```bash
ubuntu@testdevo:~$ lsblk

NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda       8:0    0    5G  0 disk
├─sda1    8:1    0    4G  0 part /
├─sda15   8:15   0   99M  0 part /boot/efi
└─sda16 259:0    0  923M  0 part /boot
vda     253:0    0   52K  1 disk
```

#### Paso 2: Verificar el Rendimiento del Sistema
- Revisa la información del CPU para entender mejor la capacidad del sistema.

```bash
top - 17:24:58 up  1:47,  1 user,  load average: 0.00, 0.01, 0.00
Tasks:  94 total,   1 running,  93 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :    953.1 total,    586.6 free,    218.6 used,    226.1 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.    734.5 avail Mem
```

- Analiza la información de la memoria para identificar posibles cuellos de botella.

```bash
ubuntu@testdevo:~$ cat /proc/meminfo
MemTotal:         975936 kB
MemFree:          600420 kB
MemAvailable:     751868 kB
Buffers:           16420 kB
Cached:           196908 kB
SwapCached:            0 kB
Active:           206656 kB
Inactive:          44824 kB
```

- Analiza los últimos 20 registros del log del sistema para confirmar que no haya nada mal con el SO.

```bash
ubuntu@testdevo:~$ tail -n 50 /var/log/syslog
2024-07-08T14:12:10.820304-03:00 testdevo systemd[1]: Finished fwupd-refresh.service - Refresh fwupd metadata and update motd.
2024-07-08T15:27:48.154186-03:00 testdevo systemd-resolved[457]: Clock change detected. Flushing caches.
2024-07-08T15:27:48.176267-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T15:27:48.189196-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T15:27:48.189511-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T15:30:03.977192-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T15:30:03.990233-03:00 testdevo systemd[1]: fwupd.service: Deactivated successfully.
2024-07-08T15:30:03.998613-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T15:30:03.999289-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T15:35:01.401763-03:00 testdevo CRON[1198]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2024-07-08T15:38:50.337984-03:00 testdevo systemd[1]: Starting fwupd-refresh.service - Refresh fwupd metadata and update motd...
2024-07-08T15:38:50.377555-03:00 testdevo dbus-daemon[622]: [system] Activating via systemd: service name='org.freedesktop.fwupd' unit='fwupd.service' requested by ':1.20' (uid=990 pid=1202 comm="/usr/bin/fwupdmgr refresh" label="unconfined")
2024-07-08T15:38:50.388028-03:00 testdevo systemd[1]: Starting fwupd.service - Firmware update daemon...
2024-07-08T15:38:50.470086-03:00 testdevo fwupd[1207]: 18:38:50.469 FuMain               Daemon ready for requests (locale C.UTF-8)
2024-07-08T15:38:50.470664-03:00 testdevo dbus-daemon[622]: [system] Successfully activated service 'org.freedesktop.fwupd'
2024-07-08T15:38:50.470690-03:00 testdevo systemd[1]: Started fwupd.service - Firmware update daemon.
2024-07-08T15:38:50.474760-03:00 testdevo systemd[1]: fwupd-refresh.service: Deactivated successfully.
2024-07-08T15:38:50.474810-03:00 testdevo systemd[1]: Finished fwupd-refresh.service - Refresh fwupd metadata and update motd.
2024-07-08T15:40:03.228688-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T15:40:03.243732-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T15:40:03.244041-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T15:43:50.969705-03:00 testdevo systemd[1]: fwupd.service: Deactivated successfully.
2024-07-08T15:45:02.028891-03:00 testdevo CRON[1226]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2024-07-08T15:50:03.279270-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T15:50:03.298111-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T15:50:03.298597-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T15:55:02.148307-03:00 testdevo CRON[1234]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2024-07-08T16:00:03.290567-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T16:00:03.304076-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T16:00:03.304336-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T17:21:58.291211-03:00 testdevo systemd-resolved[457]: Clock change detected. Flushing caches.
2024-07-08T17:21:58.304001-03:00 testdevo systemd[1]: Starting fwupd-refresh.service - Refresh fwupd metadata and update motd...
2024-07-08T17:21:58.308410-03:00 testdevo systemd[1]: Starting motd-news.service - Message of the Day...
2024-07-08T17:21:58.311268-03:00 testdevo systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2024-07-08T17:21:58.316521-03:00 testdevo systemd[1]: sysstat-collect.service: Deactivated successfully.
2024-07-08T17:21:58.317171-03:00 testdevo systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2024-07-08T17:21:58.334154-03:00 testdevo dbus-daemon[622]: [system] Activating via systemd: service name='org.freedesktop.fwupd' unit='fwupd.service' requested by ':1.22' (uid=990 pid=1242 comm="/usr/bin/fwupdmgr refresh" label="unconfined")
2024-07-08T17:21:58.341682-03:00 testdevo systemd[1]: Starting fwupd.service - Firmware update daemon...
2024-07-08T17:21:58.496642-03:00 testdevo fwupd[1259]: 20:21:58.496 FuMain               Daemon ready for requests (locale C.UTF-8)
2024-07-08T17:21:58.498403-03:00 testdevo dbus-daemon[622]: [system] Successfully activated service 'org.freedesktop.fwupd'
2024-07-08T17:21:58.498448-03:00 testdevo systemd[1]: Started fwupd.service - Firmware update daemon.
2024-07-08T17:21:58.504885-03:00 testdevo systemd[1]: fwupd-refresh.service: Deactivated successfully.
2024-07-08T17:21:58.504924-03:00 testdevo systemd[1]: Finished fwupd-refresh.service - Refresh fwupd metadata and update motd.
2024-07-08T17:21:59.779841-03:00 testdevo 50-motd-news[1290]:  * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
2024-07-08T17:21:59.780067-03:00 testdevo 50-motd-news[1290]:    just raised the bar for easy, resilient and secure K8s cluster deployment.
2024-07-08T17:21:59.780102-03:00 testdevo 50-motd-news[1290]:    https://ubuntu.com/engage/secure-kubernetes-at-the-edge
2024-07-08T17:21:59.791546-03:00 testdevo systemd[1]: motd-news.service: Deactivated successfully.
2024-07-08T17:21:59.793314-03:00 testdevo systemd[1]: Finished motd-news.service - Message of the Day.
2024-07-08T17:25:01.281612-03:00 testdevo CRON[1301]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2024-07-08T17:26:58.395716-03:00 testdevo systemd[1]: fwupd.service: Deactivated successfully.
```

#### Paso 3: Verificación y Reinicio de Servicios

- Verificar el estado del servicio Apache
```bash
ubuntu@testdevo:~$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-07-08 13:04:26 -03; 4h 24min ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 617 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 650 (apache2)
      Tasks: 55 (limit: 1059)
     Memory: 8.5M (peak: 8.7M)
        CPU: 773ms
     CGroup: /system.slice/apache2.service
             ├─650 /usr/sbin/apache2 -k start
             ├─653 /usr/sbin/apache2 -k start
             └─654 /usr/sbin/apache2 -k start

Jul 08 13:04:25 testdevo systemd[1]: Starting apache2.service - The Apache HTTP Server...
Jul 08 13:04:26 testdevo apachectl[638]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress thi>
Jul 08 13:04:26 testdevo systemd[1]: Started apache2.service - The Apache HTTP Server.
```

- Reiniciar el servicio Apache

```bash
ubuntu@testdevo:~$ sudo systemctl restart apache2
```

- Verificar los logs en /var/log/apache2/error.log para ver si todo funciona bien

```bash
ubuntu@testdevo:~$ tail -f /var/log/apache2/error.log
[Mon Jul 08 00:36:23.316457 2024] [mpm_event:notice] [pid 2703:tid 250946255450144] AH00489: Apache/2.4.58 (Ubuntu) configured -- resuming normal operations
[Mon Jul 08 00:36:23.316467 2024] [core:notice] [pid 2703:tid 250946255450144] AH00094: Command line: '/usr/sbin/apache2'
[Mon Jul 08 13:04:06.131042 2024] [mpm_event:notice] [pid 2703:tid 250946255450144] AH00492: caught SIGWINCH, shutting down gracefully
[Mon Jul 08 13:04:26.036795 2024] [mpm_event:notice] [pid 650:tid 253540839174176] AH00489: Apache/2.4.58 (Ubuntu) configured -- resuming normal operations
[Mon Jul 08 13:04:26.042415 2024] [core:notice] [pid 650:tid 253540839174176] AH00094: Command line: '/usr/sbin/apache2'
[Mon Jul 08 17:30:23.563612 2024] [mpm_event:notice] [pid 650:tid 253540839174176] AH00492: caught SIGWINCH, shutting down gracefully
[Mon Jul 08 17:30:23.621203 2024] [mpm_event:notice] [pid 1343:tid 250347643682848] AH00489: Apache/2.4.58 (Ubuntu) configured -- resuming normal operations
[Mon Jul 08 17:30:23.621262 2024] [core:notice] [pid 1343:tid 250347643682848] AH00094: Command line: '/usr/sbin/apache2'
```



