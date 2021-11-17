# TIPS UBUNTU/LINUX

## TIP 1. Cambiar password en ubuntu:

- Enciende tu PC, y cuando salga el prompt del grub presionas la tecla **ESC**

- Presiona **e** para editar

- Desplázate hasta la linea del kernel que usas en caso de que sean 2 o más y presiona **e**

- Sitúate hasta la linea del final y agrega **rw init=/bin/bash**

- Presiona **Enter** y después **b** para arrancar _(boot)_ tu sistema

- Tu sistema iniciará con el usuario **Root** y sin contraseña

- Ahora solo teclea:

passwd tu_usuario

- Escribe la contraseña que quieras poner

- Reinicia el sistema

## TIP 2. Para buscar los usuarios que tiene cron

se puede hacer un:

`ls /var/spool/cron/crontabs/`

La lista que aparezca de directorios son los usuarios q tiene crontab

Para que un usuario pueda usar el crontab, tiene que estar funcionando el crontabd y estar dicho usuario en el fichero /etc/cron.allow y no estar en /etc/cron.deny o bien no existir ninguno de los 2 ficheros.

## TIP 3. Resetear NetworkManager

`sudo /etc/init.d/NetworkManager restart`

## TIP 4. Cambiar la MAC de una interfaz de red

`sudo ifconfig eth0 down`

`sudo ifconfig eth0 hw ether [direccion mac,ej: 00:00:00:00:00 la que sea]`

`sudo ifconfig etho up`

## TIP 5. Crear ISO a partir de un CD/DVD

- Crear un ISO de un dispositivo:
  `dd if=/dev/hdX of=hdX.iso`

- Realizar copia del MBR:
  `dd if=/dev/hdX bs=512 count=1 of=mbr.iso`

## TIP 6. VER PUERTOS ABIERTOS

`netstat -npl`

## TIP 7. Sincronizar por ssh una fuente en un destino con rsync:

`rsync -av --progress --inplace --rsh=&#39;ssh&#39; SRC DEST`

## TIP 8. Montar fichero iso como carpeta

`sudo mount -t iso9660 -o loop archivo.iso /media/iso`

## TIP 9. Text to Speech o como hacer que el ordenador hable

`espeak -v en &quot;Hello i am espeak&quot;`

`espeak -v es &quot;Hola soy espeak&quot;`

## TIP 10. Añadir un usuario existente a un grupo existente

`sudo gpasswd -a manu www-data`

## TIP 11. Cambiar color de la consola

`tput setf 1`

1 azul osc, 2 verde, 3 azul claro, 4 rojo, 5 violeta, 6 amarillo, 7 y 8 blanco brillante, 9 blanco opaco

## TIP 12. Escaneo de los puertos de la red 192.168.0.X

`nmap -v -sT 192.168.0.0/24`

## TIP 13. Instalar DVD:RIP

```
sudo apt-get install dvdrip libdvdread4 rar
sudo /usr/share/doc/libdvdread4/install-css.sh
```

## TIP 14. Obtener los UUID de los discos duros

`ls -l /dev/disk/by-uuid`

## TIP 15 Como saber el Hardware que tiene tu equipo

`sudo lshw -sanitize`

## TIP 16 Como saber el tipo de memoria que tiene tu PC

`sudo dmidecode --type 17`

## TIP 17 Como montar una unidad por ssh

```
mkdir /media/unidad_ssh
sudo chown tu_usuario.tu_usuario /media/unidad_ssh
sudo apt-get install sshfs
sshfs usuario@servidor:/path/carpeta/remota /media/unidad_ssh
```

Para montarlo automaticamente en fstab y usando un par de claves RSA:

`sudo vi /etc/fstab`

Y añadir al final (pulsar G y después o):

`sshfs#username@miservidor:/ruta/remota /mnt/micarpeta/ fuse comment=sshfs,noauto,users,exec,reconnect 0 0`

(guardar y cerrar con :wq! )

```
ssh-keygen -t rsa (si ya tienes generadas tus claves no hace falta esto)
ssh-copy-id -i ~/.ssh/id_rsa.pub usuario@servidor
mount /mnt/micarpeta
```

## TIP 18. Mover el directorio /home a otra partición

Entrar como root:

su o sudo su

Crear nueva partición y montarla:

`mount /dev/sda3 /data1`

Ir al directorio /home y copiar todo en /data1

`cd /home`

`cp -ra \* /data1/`

Editar /etc/fstab y añadir el nuevo punto de montaje:

```
#HOME
/dev/sda3 /home defaults 1 2
```

Renombrar /home a /oldhome

`mv /home /oldhome`

Reiniciar

## TIP 19. Formatear en ext4

`mkfs.ext4 /dev/sdb1`

## TIP 20. Si sale el mensaje:`_no se puede usar `inotify&#39;, se vuelve a polling*&#39; o &#39;\_inotify cannot be used, reverting to polling*&#39;

Editar:

`sudo vi /etc/sysctl.conf`

Añadir al final (o editar si existiese):

`fs.inotify.max_user_watches = 1048576`

Y si sigue fallando hacer el tail como root...

## TIP 21. Chequear disco duro

`fsck /dev/sda1`

## TIP 22. Escribir imagen .iso/.img en memoria SD

`dd bs=4M if=/ruta/a/archlinux.iso of=/dev/sdx`

## TIP 23. Listado de particiones

`lsblk`

`df`

`sudo fdisk -l`

## TIP 24. Cambiar nombre del equipo

`sudo hostname _nuevo_nombre_`

`sudo vi /etc/hostname y cambiar el nombre por _nuevo_nombre_`

## TIP 25. Mover los archivos de muchos directorios a un solo directorio

`cd directorio_con_muchos_directorios_con_ficheros`

`find . -type f -exec mv &#39;{}&#39; /destino/un.solo.dir/ \;`

## TIP 26. Restaurar a eth0 cuando aparece como eht3,4,5...

Editar `/etc/udev/rules.d/70-persistent-net.rules` y borrar las entradas.

Al parecer cuando cambiamos la tarjeta de red o la MAC si es un virtualbox linux asocia el siguiente ethX a la nueva MAC.

## TIP 27. Cambiar prompt

Editar `~/.bashrc`

Añadir al final:

`PS1=&#39;${debian\_chroot:+($debian_chroot)}\[\033[01;32m\]\u\033[01;34m@\033[01;31m\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$\&gt;&#39;`

Con colores:

```
red=$(tput setaf 1)
green=$(tput setaf 2)
yellow=$(tput setaf 3)
blue=$(tput setaf 4)
magentaF=$(tput setaf 5)
magenta=$(tput setab 5)
cyanF=$(tput setaf 6)
cyan=$(tput setab 6)
white=$(tput setab 7)
bold=$(tput bold)
reset=$(tput sgr0)
export PS1=&#39;\[$blue\][$(date +%k:%M:%S)]\[$reset\]\[$yellow$bold\]\u@\[$red$bold\]\h\[$reset\]:\[$cyanF$bold\]\w\[$reset\]\&#39;
```

## TIP 28. Crear copia de seguridad de memoria SD

Haciendo:

```
sudo fdisk -l
```

Obtenemos la lista de particiones de la SD. En mi caso /dev/mmcblk0p1 y p2

```
sudo umount /dev/mmcblk0p1
sudo umount /dev/mmcblk0p2
sudo dd if=/dev/mmcblk0 of=cp_seg.img
```

## TIP 29. Montar sistema de ficheros SAMBA

```
sudo aptitude install smbfs
sudo aptitude install cifs-utils
sudo mkdir /mnt/compartida
sudo mount -t ~~smb~~ cifs -o username=&#39;usuario&#39;,password=&#39;clave&#39;,workgroup=grupowin //equipowin/compartida /mnt/compartida
```

OPCION 2: ( MEJOR )

```
sudo apt-get install cifs-utils
sudo mkdir /mnt/ejemplo
sudo vi /etc/fstab
```

Añadir y guardar:

```
//1.2.3.4/ejemplo /mnt/ejemplo cifs uid=pi,gid=pi,file_mode=0644,dir_mode=0755,noexec,credentials=/etc/cifs.credentials 0 0
```

```
\\192.168.1.9\MIDISCO /media/MIDISCO cifs defaults,uid=1000,gid=1000,file_mode=0777,dir_mode=0777,credentials=/etc/cifs.credentials 0 0
```

`sudo vi /etc/cifs.credentials`

Añadir y guardar:

```
user=SAMBA_USER
password=SAMBA_PASSWORD
```

Para cambiar/configurar la password de samba:

```
sudo smbpasswd -a \&lt;user\&gt;
```

## TIP 30. Montar disco USB con permisos 777 (lectura, escritura y ejecución para todos)

Conectar el disco duro USB.

Ejecuta:

`sudo blkid`

Copia el id de tu unidad USB

Edita fichero fstab:

`sudo vi /etc/fstab`

Añade al final:

`UUID=el_uuid_del_usb /media/dondelomontes vfat rw,users,utf8,umask=000 0 0`

Guardar y salir ( ESC :wq! )

Desmonta disco USB:

`sudo umount /dev/sdd1`

Monta disco USB:

`sudo mount /dev/sdd1 (por ejemplo)`

## TIP 32. Hacer que se ejecute un script al inicio con permisos de root

```
cd /etc/init.d
sudo ln -s ~/bin/dropbox_max_users dropbox_max_users
sudo update-rc.d dropbox_max_users defaults 80
cd /etc/rc2.d/
sudo ln -s /etc/init.d/dropbox_max_users /etc/rc2.d/S **80** dropbox_max_users
```

## TIP 33. Cambiar color del prompt

`sudo vi .bashrc`

(Añadir al final)

```
red=$(tput setaf 1)
green=$(tput setaf 2)
yellow=$(tput setaf 3)
blue=$(tput setaf 4)
bold=$(tput bold)
reset=$(tput sgr0)
export PS1=&#39;\[$red\][$(date +%k:%M:%S)]\[$yellow$bold\]\u@\[$green$bold\]\h\[$reset\]:\[$blue$bold\]\w\[$reset\]\&#39;
```

## TIP 34. Ver documentos en la cola de la impresora

`lpstat -o|more`

Para cancelarlos

`cancel {pid number}`

## TIP 35. Reiniciar las X-Windows

Si se quedan colgadas o no responden primero iremos con CTRL + ALT + F1 a la consola y nos logaremos.

Después podemos intentar:

```
sudo services ligthdm stop o sudo services gdm stop ( según tengamos )
sudo services ligthdm start o sudo services gdm start
```

Podemos repetirlo un par de veces si no funciona a la primera.

Si no podemos intentar:

`sudo xkill -all`

## TIP 36. Montar swap en otra unidad

Crear la unidad o espacio para la swap y después

`sudo swapon /dev/sdb1`

## TIP 37. Recuperar el arranque (GRUB)

Si después de trastear con nuestros discos duros, o simplemente al arrancar nuestro sistema nos aparece:

`grub rescue>`

Es que el sistema de arranque tiene algo mal.

Te recomiendo que lo reinicies un par de veces para asegurarnos que el arranque está mal. A veces pasa una vez y no vuelve a pasar…

Antes de nada también necesitamos saber los tipos de nuestros discos duros, si son SATA o PATA.

Lo podemos averiguar abriendo el PC y mirando el cable que sale del disco duro. Si es un cable muy ancho, con muchos hilos juntos, es PATA. Si es un cable como un dedo de ancho, que suele ser de color negro o rojo, es SATA. Nos quedamos con este dato.

Ahora vamos a restaurar el arranque.

Mostramos el listado de las particiones:

`grub rescue\&gt; ls`

Nos aparecen las particiones disponibles, por ejemplo:

(hd0)(hd0,1)(hd1)(hd1,1)(hd1,5)(hd2)(hd2,1)(hd3)(hd3,1)

Tenemos que descubrir que partición tiene la carpeta de /boot así que hacemos ls con cada partición para averiguarlos:

```
ls (hd0,1)/
ls (hd1,1)/
ls (hd1,5)/

[...]
```

En cuanto veamos en el listado la carpeta /boot paramos y recordamos la unidad que la contiene.

Así mismo también debemos recordar el kernel, que más adelante necesitaremos saber. En ese listado donde está el /boot buscamos un fichero vmlinuz dentro de /boot

`grub rescue\&gt; ls (hd3,1)/boot/`

Lo anotamos o recordamos.

Continuamos con:

`grub rescue\&gt;set prefix=(hd3,1)/boot/grub`

En mi ejemplo la partición que tiene el /boot es la (hd3,1). Tu tendrías que poner la que se adecúe en tu caso.

Seguimos con:

```
grub rescue\&gt; insmod (hd3,1)/boot/grub/linux.mod
grub rescue\&gt;set root=(hd3,1)
```

Para la siguiente línea necesitamos el kernel (núcleo del sistema operativo).

Para saber la UNIDAD si el disco duro es SATA usamos sd y si es PATA ponemos hd.

Añadimos el primer número que va junto al hd en (hd3,1) como letra ( 0=a, 1=b, 2=c…) y seguido del segundo número. En mi caso d3. Así que mi **UNIDAD** queda como sdd3

Resumiendo en mi caso **KERNEL** : vmlinuz-3.13.0-45-generic y **UNIDAD** sdd3.

`grub rescue\&gt; linux /boot/ **vmlinuz-3.13.0-45-generic** root=/dev/ **sdd3**`

Continuamos cargando el kernel:

`grub rescue\&gt; initrd /initrd.img`

Y reiniciamos a ver si funciona todo:

`grub rescue\&gt; boot`

Si arrancó bien es recomendable reinstalar el grub desde el propio sistema con:

`sudo grub-install /dev/sdd3(sdd3 es mi unidad, recuerda)`

Reinstalamos todos los sistemas operativos si tenemos Windows también:

`sudo update-grub`

Y con esto quedaría solucionado.

## TIP 38. Añadir variable de entorno global

Por ejemplo si queremos añadir al sistema un nuevo PATH:

`sudo vi /etc/environment`

Añadimos al final:

`export PATH=$PATH:/mi/nueva/ruta/global`

Guardamos.
