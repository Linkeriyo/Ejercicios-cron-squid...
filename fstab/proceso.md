# Particiones con fstab

## Disco A

~~~
fdisk /dev/sdb
~~~

Output: 
~~~
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 1
First cylinder (1-5580, default 1):
Using default value 1
Last cylinder or +size or +sizeM or +sizeK (1-558, default 5580): +10000M
~~~

Introducimos `n` para crear una nueva partición, especificamos que es primaria con `p`,
`1` para especificar que es la partición 1, damos enter para utilizar el primer sector
por defecto y finalmente la longitud en K o en M.

Para hacer otra partición repetimos el paso anterior por cada partición que queremos crear.
En este caso vamos a hacer 2 particiones.

Especificamos el tipo de sistema de archivos que tendrá cada partición con el comando `t`.

~~~
Partition number (1-4) : 1
Hex code (L to list codes): 83 
~~~

Guardamos los cambios con `w`:

Para crear el filesystem se usa el comando mkfs
(/dev/sdb1 es la partición primaria 1 de ese disco).

Haremos esto con cada uno de las particiones, en el primer caso con ext4, en el segundo con fat

~~~
mkfs -t ext4 /dev/sdb1
mkfs -t vfat /dev/sdb2
~~~

Solo hay que crear un directorio que es donde posteriormente montaremos el nuevo dispositivo. 
lo crearemos en la raíz para identificarlo mas fácil.

~~~
mkdir /linux
mkdir /fat
~~~

Creadas las particiones vamos a modificar el archivo `/etc/fstab` para asignar los directorios.

~~~
nano /etc/fstab
...
/dev/sdb1   /linux    ext4      defaults    2    1
/dev/sdb2   /fat      vfat      defaults    2    1
...
:wq
~~~

Lo último que tenemos que hacer para poder usar el disco duro es montarlo, para ello
utilizaremos: 

~~~
mount /linux
mount /fat
~~~

## Disco B

Repetimos los pasos anteriores pero crearemos 3 particiones de forma que nuestro archivo
`/etc/fstab` quedará así:

~~~
...
/dev/sdc1   /linux    ext4      defaults    2    1
/dev/sdc2   /ntfs     ntfs-3g   defaults    2    1
/dev/sdc3   /fat      vfat      defaults    2    1
...
:wq
~~~

Para que se pueda crear un filesystem con ntfs debemos instalar el paquete `ntfs-3g`.

Montamos cada uno de las particiones y ya está hecho.