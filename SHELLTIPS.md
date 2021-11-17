# SHELL TIPS

## TIP 1. Cómo reemplazar cadenas en ficheros

Para reemplazar una cadena por otra en todos los fichero q cumplan el patron, incluidos subdirectorios se procede como sigue:

```
cd Directorio_a_partir_de_donde_buscar
find . -type f -name &quot;\*.txt&quot; -exec sed -i &quot;s/cadena_buscada/nueva_cadena/g&quot; {} \;
```

## TIP 2. Búsquedas y acciones en el sistema de ficheros

#### Buscar y borrar directorios vacíos (cuidado con los del wine y otros)

```
find -type d -empty -print0 | xargs -0 rmdir
```

#### Buscar y borrar ficheros que tengan más de 90seg de antigüedad

```
find -type f -mtime +90 -exec file \{\} \;
```

#### Buscar y mostrar información de ficheros que ocupen más de 100 Mbytes en disco

```
find -type f -size +100M -exec file \{\} \;
```

#### Borrar directorios CVS recursivamente

```
find -type d -name CVS -print0 | xargs -0 rm -rf
```

#### Encontrar ficheros .jpg y cambiarles el tamaño a máximo 600 de ancho y renombrarlo con el mismo nombre terminado en 2

```
find . -type f -name &#39;\*.jpg&#39; -exec sh -c &#39;convert -resize 600x &quot;$0&quot; &quot;${0%.\*}2.jpg&quot;&#39; {} \;
```

## TIP3. Pasar de ogv a avi

```
mencoder the_reef.ogv -ovc xvid -oac mp3lame -xvidencopts pass=1 -o the_reef.avi
```

## TIP 4. Descomprimir multiples .rar

```
find -type f -name &#39;\*.rar&#39; -exec unrar x {} \;
```

## TIP 5. Unir fichero partidos con Hacha o con split:

```
cat mi_archivo.part\*.\* \&gt; mi_archivo.\*
```

## TIP 6. Obtener los UUID de los discos duros

```
ls -l /dev/disk/by-uuid
```

## TIP 7. Como saber el Hardware que tiene tu PC

```
sudo lshw -sanitize
```

## TIP 8. Como saber el tipo de memoria que tiene tu PC

```
sudo dmidecode --type 17
```

## TIP 9. Chequear disco duro

```
fsck /dev/sda1v
```

## TIP 10. Escribir imagen .iso/.img en memoria SD

```
dd bs=4M if=/ruta/a/archlinux.iso of=/dev/sdx
```

## TIP 11. Listado de particiones

```
lsblk
```

o

```
df
```

o

```
sudo fdisk -l
```

## TIP 12. Mover los archivos de muchos directorios a un solo directorio

```
cd directorio_con_muchos_directorios_con_ficheros
find . -type f -exec mv &#39;{}&#39; /destino/un.solo.dir/ \;
```

## TIP 13. Eliminar recursivamente un directorio (p.Ej. CVS)

```
rm -rf `find . -type d -name .CVS`
```

## TIP 14. Ver documentos en la cola de la impresora

```
lpstat -o|more
```

Para cancelarlos

```
cancel {pid number}
```

## TIP 15. BOMBA FORK

¡¡¡OJO CUIDAO!!!
Crea infinitos fork hasta colapsar el sistema:

```
:(){:|:&amp;};:
```

## TIP 16. PASAR TEXTO A TEXTO ASCII GIGANTE

```
sudo apt-get install figlet
filget FIGLET
```

O también con:

```
sudo apt-get install sysvbanner
banner BANNER
```

## TIP 17. Buscar fichero recursivamente dentro de directorios

```
ls -lR | egrep &quot;^\.\/|NOMBRE_FICHERO.KSEA&quot;| more
```

## TIP 18. Convertir varios fichero JPG en un fichero PDF

```
convert imagen1.jpg imagen2.jpg imagen3.jpg archivo.pdf
```

## TIP 19. Renombrar masivamente ficheros:

#### Renombrar imagenXXXX.jpg por imagen_NEW_XXXX.jpg

```
rename -v -n &#39;s/imagen/imagen_NEW\_/&#39; \*.jpg
```

#### Renombrar nombreimagen.png por nombreimagen_150x150.png

```
rename -v -n &#39;s/\.png/\_150x150.png/&#39; \*.png
```

#### Renombrar cambiando \_ por -

```
rename -v &#39;s/\_/-/&#39; \*.jpg **(añadiendo -n te muestra lo que cambiará sin hacerlo)**
```

#### Renombrar los ficheros tipo:

leccion 1.doc,
leccion 2.doc ...

por

tema - leccion 1.doc,
tema - leccion 2.doc ...

```
rename -v -n &#39;s/^/tema – /&#39; \*.doc
```

#### Renombrar los ficheros tipo:

texto1_abc_001_small.jpg,
texto2_abc_002_small.jpg,
texto3_abc_003_small.jpg

por

texto1_small.jpg,
texto2_small.jpg,
texto3_small.jpg

```
rename -v -n &#39;s/\w{8}\_small/\_small/&#39; \*.jpg
```
