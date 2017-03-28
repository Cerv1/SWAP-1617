# Servidores Web de Altas Prestaciones
## Práctica 2. Clonar la información de un sitio web.

Para esta práctica deberemos resolver 4 apartados.

### 1. Copia de archivos mediante ` ssh `

Como primera aproximación a la idea de tener el contenido replicado en ambas máquinas podemos utilizar este proceso, bastante sencillo a la par que ineficiente.

Lo que haremos en este apartado será generar un archivo con extensión **.tgz** que contendrá toda la información que queramos que sea replicada y mediante ` ssh ` podremos enviarlo a la otra máquina. Para realizar esta tarea deberemos utilizar la siguiente orden:

``` tar zcvf - /var/www/ | ssh user@ip-dest "cat > ~/bk.tgz" ```

Con esta orden copiaremos el directorio ` /var/www/ ` ya comprimido en el home de la máquina destino. Pero como hemos comentado, tiene un grave problema y es que cada vez que queramos hacer esto deberemos ingresar la contraseña de ssh. Además, si el archivo fuera bastante grande, ocuparíamos el ancho de banda durante un buen rato, disminuyendo la calidad del servicio que podemos ofrecer.

Aquí podemos comprobar que funciona:

![imagen-TAR-SSH]()
