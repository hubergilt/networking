# Networking

Laboratorio 1 : Construyendo una red IPv4 ...

El diseño de una red IPv4, inicia conociendo la cantidad de miembros pertenecientes a la misma (llamados hosts), luego con la selección de un rango apropiado de direcciones IP (existen los rangos de Clase A, B, C, D y E).

El ejemplo más sencillo es una red punto a punto, que tiene dos miembros y puede ser una de las distintas redes privadas: 

La clase D, es para el uso en multicast (un grupo de miembros que tiene asociado una dirección IP) y la clase E, son para fines experimentales.

Red privada | Rango Inicial | Rango Final       | Red punto a punto |
------------|---------------|-------------------|-------------------|
Clase A     | 10.0.0.0/24   | 10.255.255.255/24 | 10.0.0.0/30       |
Clase B     | 172.16.0.0/12 | 172.31.255.255/12 | 172.16.0.0/30     |
Clase C     | 192.168.0.0/8 | 192.168.255.255/8 | 192.168.0.0/30    |


Desarrollando para el caso de un red privada clase A, tenemos las direcciones:
* Dirección de red : 10.0.0.0
* Direccion del primer extremo : 10.0.0.1
* Dirección del otro extremo : 10.0.0.2
* Dirección de broadcast : 10.0.0.3

Utilizamos el simulador de red "packetracer", contruir y probar la red punto a punto clase A.

## Configuración de la direción IP en el Router A
```
enable 
configure terminal
 interface Se0/1/0
  ip address 10.0.0.1 255.255.255.252
  no shutdown
   exit
 exit
write mem
```

## Configuración de la direción IP en el Router B
```
enable 
configure terminal
 interface Se0/1/0
  ip address 10.0.0.2 255.255.255.252
  no shutdown
  exit
 exit
write mem
```
## Gráfica de red privada de punto a punto clase A

![Gráfica de red privada de punto a punto clase A](lab1/p2p.png "Gráfica de red privada de punto a punto clase A")
