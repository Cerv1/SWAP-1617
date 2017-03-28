# Servidores Web de Altas Prestaciones
## Práctica 1. Preparación de las herramientas

Lo primero que realizaremos en esta sencilla práctica es la instalación de las dos máquinas virtuales. Para ello lo único que debemos hacer es seguir el guión de la práctica hasta tener ambas máquinas funcionando con LAMP. Una muestra de que ambas máquinas están funcionando:

![imagenLAMP](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%201/apache2-vms.png)

Una vez tenemos ambas máquinas con el servicio **Apache** corriendo modificamos la página web que muestra por defecto el servidor, situada en la ruta ``` /var/www/html/index.html ``` y la cambiamos por una sencilla página que lo único que haga sea mostrar si es la máquina número 1 o la máquina número 2.

Para comprobar que todo funciona correctamente utilizamos el comando **curl** mediante la siguiente orden:
``` curl http://ip-maquina/ ```.

Lo realizamos desde ambas máquinas hacia la otra para comprobar que todo funciona correctamente:

![imagenCURL](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%201/curl-vms.png)
