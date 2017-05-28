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

### 3. Restaurar dicha copia en la segunda máquina (clonado manual de la BD).
### 4. Realizar la configuración maestro-esclavo de los servidores MySQL para que la replicación de datos se realice automáticamente.
