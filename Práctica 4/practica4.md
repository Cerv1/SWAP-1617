# Servidores Web de Altas Prestaciones
## Práctica 3. Balanceo de carga.

Para esta práctica, deberemos resolver 2 apartados.

### 1. Instalar un certificado SSL para configurar el acceso HTTPS a los servidores.

Para este apartado deberemos generar e instalar un certificado SSL autofirmado. Para ello, lo primero que deberemos hacer es activar el módulo SSL de Apache, generarlos y especificarle la ruta. Para ello ejecutamos las siguientes líneas:

```
a2enmod ssl
service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
```

Como podemos comprobar, la primera línea se encarga de activar el módulo, la segunda reinicia el servicio para que esté activado el módulo y las líneas tercera y cuarta indica dónde está el certificado recién generado.

Esto nos pedirá que ingresemos algunos campos tales como el código del país, correo de contacto, provincia, etc... Tan solo deberemos de rellenarlo de una manera lógica.


### 2. Configurar las reglas del cortafuegos para proteger la granja web.

La mejor opción es configurar nuestro firewall mediante el siguiente script, que usa la orden `iptables`.
```
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

iptables -A INPUT -p tcp --dport 22  -j ACCEPT
iptables -A INPUT -p tcp --dport 80  -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

iptables -A OUTPUT -p tcp --sport 22  -j ACCEPT
iptables -A OUTPUT -p tcp --sport 80  -j ACCEPT
iptables -A OUTPUT -p tcp --sport 443 -j ACCEPT
```
