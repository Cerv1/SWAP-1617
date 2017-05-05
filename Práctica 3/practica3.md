# Servidores Web de Altas Prestaciones
## Práctica 3. Balanceo de carga.

Para esta práctica, deberemos resolver 3 apartados.

### 1. Configurar una máquina e instalarle `nginx` como balanceador de carga.

Lo único que tendremos que hacer es instalar el programa en la máquina balanceadora con la configuración que nos proporciona el guión de la práctica. Un ejemplo de que todo está funcionando correctamente:

![imagen-nginx](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%203/nginx-balancer.png)

En este caso ambas máquinas se repartirían la carga de manera equitativa. Pero el enunciado también nos pide que la carga se distribuya mediante el algoritmo **Round-Robin** y para ello tendremos que añadir las siguientes líneas al archivo de configuración de `nginx`:

![imagen-round-robin](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%203/round-robin.png)



### 2. Configurar una máquina e instalarle `haproxy` como balanceador de carga.

Para esta parte, de nuevo, tan sólo tendremos que seguir el guión y comprobar que todo funciona correctamente.

![imagen-haproxy](https://github.com/Cerv1/SWAP-1617/blob/master/Pr%C3%A1ctica%203/haproxy.png)
