# Servidores Web de Altas Prestaciones
## Práctica 5. Replicación de bases de datos MySQL.

Para esta práctica, deberemos resolver 4 apartados.

### 1. Crear una BD con al menos una tabla y algunos datos.

Lo primero que deberemos hacer en esta práctica es crear una base de datos MySQL e insertar algunos datos para después poder hacer las copias de seguridad y comprobar que todo está funcionando correctamente.

Para comenzar deberemos entrar en el prompt de MySQL mediante el comando `mysql -u root -p` e introducir nuestra contraseña de usuario **root**.

Una vez estamos dentro, crearemos la base de datos con la orden `create database <nombre>`, en nuestro caso la BD se llamará **SWAP**.

Lo siguiente que realizaremos será usar esta base de datos con `use SWAP`. Si ejecutamos el comando `show tables` nos dirá que no hay ninguna pues la BD acaba de ser creada y aún no contiene ninguna tabla. Para crearla ejecutaremos `create table <nombre>(<param> <type>, ...)` y en nuestro caso se llamará **datos** y tendrá como atributos el *nombre*, una variable de hasta 100 caracteres, y *tlfn*, un entero.

Una vez tenemos la tabla creada, probaremos a introducir algún dato para que no esté vacía mediante `insert into datos (<param-1>, <param-2>, ...) (<value-1>, <value-2>, ...)`. Una vez tenemos insertados los datos podemos comprobar que se han insertado correctamente ejecutando `select * from datos` y, como es lógico, nos mostrará el contenido de dicha tabla. Además hemos añadido una orden que nos muestra algo más de información sobre la BD.

![select-all](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%205/select-all.png)

### 2. Realizar la copia de seguridad de la BD completa usando `mysqldump`.

Este paso es bastante sencillo. Lo primero es evitar que en el servidor se accedan y se cambien las BD mientras hacemos la copia de seguridad y para ello ejecutamos `FLUSH TABLES WITH READ LOCK` y acto seguido realizamos la copia mediante el siguiente comando `mysqldump <database> -u root -p > /dir-to-backup/backup.sql` y como podemos comprobar, ya tendríamos hecha nuestra copia de seguridad de la base de datos.

![mysqldump](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%205/mysqldump.png)

Después de realizar esta copia **desbloqueamos las tablas** para que puedan volver a ser utilizadas con el comando `unlock tables`.

### 3. Restaurar dicha copia en la segunda máquina (clonado manual de la BD).

Una vez realizada la copia de seguridad de nuestra BD, podemos clonar manualmente la BD a nuestra máquina de respaldo de distintas maneras, tal y como vimos en la [práctica 2](https://github.com/Cerv1/SWAP-1617/tree/master/Pr%C3%A1ctica%202). Cabe mencionar que para que esto funcione, la máquina de respaldo tiene que tener creada la BD ya que mysqldump contiene sentencias para rellenar la BD en el otro equipo pero no se encarga de crear la tabla. Para realizar la copia me decantaré por la opción del **pipe + ssh** ejecutando `mysqldump <database> -u root -p | ssh <user@ip-destino> mysql`y podemos comprobar que funciona en la siguiente imagen:


### 4. Realizar la configuración maestro-esclavo de los servidores MySQL para que la replicación de datos se realice automáticamente.

Como es obvio, lo realizado en el apartado anterior funciona pero no es lo óptimo. Por tanto, vamos a realizar una configuración **maestro-esclavo** y que este proceso se haga automáticamente. Para ello, lo primero de todo es editar el archivo `/etc/mysql/mysql.conf.d/mysqld.cnf` y realizar las siguientes modificaciones en la máquina maestro:

1. Comentar la línea `bind-address 127.0.0.1` para que escuche al servidor.
2. Añadir `log_error = /var/log/mysql/error.log` para mantener un registro de los errores.
3. Añadir `server-id = 1` para establecer el identificador del servidor.
4. Añadir `log_bin = /var/log/mysql/bin.log` para obtener un formato más eficiente y más seguro para las transacciones.

Después reiniciamos el servicio con `/etc/init.d/mysql restart` y si no sale ningún tipo de error ya habremos configurado el maestro.

La configuración del **esclavo** es igual, lo único que deberemos de cambiar es el `server-id` que en la máquina maestro es **1** y en esta será **2**. Una vez realizados los cambios reiniciamos también el servicio.

El siguiente paso será crear un usuario en el maestro y darle permisos de replicación, para ello ejecutamos las siguientes sentencias `sql`:
```sql
CREATE USER esclavo IDENTIFIED BY 'esclavo';
GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
FLUSH PRIVILEGES;
FLUSH TABLES;
FLUSH TABLES WITH READ LOCK;
```


Una vez hecho esto deberemos comprobar el **MASTER_LOG_FILE** y el **MASTER_LOG_POS** mediante la orden `SHOW MASTER STATUS`. Una vez anotamos estos valores solo nos quedará ejecutar la siguiente orden en la máquina esclava:

```sql
CHANGE MASTER TO MASTER_HOST='<ip-master>', MASTER_USER='esclavo',
MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='<log-file>',
MASTER_LOG_POS=<position>, MASTER_PORT=3306;
```

y desde eeste momento todas las modificaciones realizadas en la máquina maestra serán replicadas en la máquina esclava como podemos comprobar:

![slave-master](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%205/slave-master.png)
