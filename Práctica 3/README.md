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

### 3. Comparación de prestaciones entre ambos balanceadores.

Para poder decir que uno es mejor que otro vamos a someter al balanceador a una alta carga de peticiones y tomaremos los tiempos, de esta manera, podremos ver si hay un claro ganador.

La orden que utilizaremos para comparar ambos balanceadores será:

`ab -n 50000 -c 120 http://ip-balanceador/index.html`

Los resultados han sido:

| nginx  | haproxy |
| -----  | ------- |
| 29.275 | 28.551  |
| 28.989 | 29.650  |
| 29.644 | 28.920  |

A pesar de haber sometido a una alta carga, los resultados son muy parecidos en ambos. Por tanto, no podemos decir que uno sea claramente mejor que otro.
