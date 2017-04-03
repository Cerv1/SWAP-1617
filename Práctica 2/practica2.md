# Servidores Web de Altas Prestaciones
## Práctica 2. Clonar la información de un sitio web.

Para esta práctica deberemos resolver 4 apartados.

### 1. Copia de archivos mediante ` ssh `

Como primera aproximación a la idea de tener el contenido replicado en ambas máquinas podemos utilizar este proceso, bastante sencillo a la par que ineficiente.

Lo que haremos en este apartado será generar un archivo con extensión **.tgz** que contendrá toda la información que queramos que sea replicada y mediante ` ssh ` podremos enviarlo a la otra máquina. Para realizar esta tarea deberemos utilizar la siguiente orden:

` tar zcvf - /var/www/ | ssh user@ip-dest "cat > ~/bk.tgz" `

Con esta orden copiaremos el directorio ` /var/www/ ` ya comprimido en el home de la máquina destino. Pero como hemos comentado, tiene un grave problema y es que cada vez que queramos hacer esto deberemos ingresar la contraseña de ssh. Además, si el archivo fuera bastante grande, ocuparíamos el ancho de banda durante un buen rato, disminuyendo la calidad del servicio que podemos ofrecer.

Aquí podemos comprobar que funciona:

![imagen-TAR-SSH](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%202/copy-tar-ssh.png)

### 2. Clonar una carpeta entre las dos máquinas.

Ahora realizaremos copias incrementales del directorio que queramos desde la máquina principal hacia la máquina de respaldo. Para ello utilizaremos la herramienta mencionada en el guión, `rsync`. Su uso es muy sencillo, tan solo deberemos de ejecutar el siguiente comando en la máquina de respaldo:`rsync -avz -e ssh ip-dest:/var/www/ /var/www/`

De esta manera, sustituirá todo el contenido de esta carpeta por lo que haya en la máquina principal.

**Nota:** Si da error con los permisos, deberemos de ingresar el siguiente comando `sudo chown user:user -R /var/www/`. De esta manera seremos los dueños de esta carpeta y no habrá ningún inconveniente.

Ejemplo de uso en el que clonamos el contenido de la máquina 1 en la máquina 2.
![imagen-RSYNC](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%202/rsync-example.png)


### 3. Configuración de `ssh` para acceder sin que solicite contraseña.

Como es lógico, no tiene mucho sentido automatizar las copias de seguridad de nuestros directorios si vamos a tener que estar pendientes de las máquinas y poniendo la contraseña cada vez que vayamos a realizar una. Para solucionar este problema podemos configurar `ssh` para que funcione a través de un esquema de **llave pública/privada**.

Para configurarlo deberemos de seguir estos pasos en la máquina de respaldo:

1. Creación de las claves mediante la orden `ssh-keygen -b 4096 -t rsa`.
2. Copia de la clave a la máquina principal mediante la orden `ssh-copy-id user@ip-dest`.

Ejemplo de su uso:

![imagen-SSH](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%202/ssh-no-password.png)
