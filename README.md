# Networking

## Laboratorio 1 : Construyendo y configurando una red de enlace IPv4

El diseño de una red IPv4, inicia conociendo la cantidad de miembros pertenecientes a la misma (llamados hosts), luego con la selección de un rango apropiado de direcciones IP (existen los rangos de Clase A, B, C, D y E).

El ejemplo más sencillo es una red es del tipo punto a punto, la que cuenta con solo dos miembros y pueden de distintos tipos de clase A, B y C.

La clase D, es para el uso en multicast (un grupo de miembros que tiene asociado una dirección IP) y la clase E, son para fines experimentales.

Red privada | Rango Inicial | Rango Final       | Red punto a punto |
------------|---------------|-------------------|-------------------|
Clase A     | 10.0.0.0/24   | 10.255.255.255/24 | 10.0.0.0/30       |
Clase B     | 172.16.0.0/12 | 172.31.255.255/12 | 172.16.0.0/30     |
Clase C     | 192.168.0.0/8 | 192.168.255.255/8 | 192.168.0.0/30    |


Desarrollando para el caso de un red privada clase A, tenemos dos direcciones de miembros (hosts) y sus correspondientes direcciones de red y de difusión:

* Dirección de red : 10.0.0.0
* Direccion del primer extremo : 10.0.0.1
* Dirección del otro extremo : 10.0.0.2
* Dirección de broadcast : 10.0.0.3

Utilizamos el simulador de red "packetracer", para construir, configurar y probar la red punto a punto clase A.

### Configuración de la direción IP en el Router A
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

### Configuración de la direción IP en el Router B
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
### Gráfica de red privada de punto a punto tipo clase A

![Gráfica de red privada de punto a punto clase A](lab1/p2p.png "Gráfica de red privada de punto a punto clase A")

Nota: Para diseñar una red de otro tamaño, es necesario conocer el numero de miembros de esta, que sea menor a una potencia de dos. Es decir, el numero de mienbros debe ser menor a 4, 8, 16, etc. Asi mismo, se debe considerar restar 2 para reservar las direcciones de red y de difusión.

## Laboratorio 2 : Agregando una red local IPv4

Continuando con el laboratorio vamos analizar la red local, para esta parte de la red usaremos las direciones de clases C, considerando que tenemos 100 miembros dentro de la red, hacemos corresponder al menor numero que sea potencia de dos (2, 4, 8, ...). Para este ejemplo es 128, pues es el mas cercano al numero 100. Dado que 128 miembros se puede representar con 7 bits en binario.  
Las direcciones privadas de clase C, tiene 24 bits para redes y 8 bits para hosts, dado que solo necesitamos 7 bits de los 8 bits para hosts, el bit restantes ahora formar parte de la direccion de red, o para ser más preciso es parte de la dirección de subred, que ahora tiene 25 bits en luegar de 24 bits y esto se representa, asi:

* Dirección de red (network)          : 192.168.0.0/25
* Direccion del primer miembro (host) : 192.168.0.1
* Dirección del ultimo miembro (host) : 192.168.0.126
* Dirección de difusión (broadcast)   : 192.168.0.127

Ahora, utilizamos el simulador para red "packetracer", para construir, configurar y probar la red privada local de clase C.

### Configuración de la direción IP en el Router A
```
enable 
configure terminal
 interface Gig0/0
  ip address 192.168.0.1 255.255.255.128
  no shutdown
  exit
 exit
write mem
```

### Configuración de la direción IP en el Switch 0 (opcional)
```
enable 
configure terminal
 interface Gig0/2
  no shutdown
  exit
 exit
write mem
```

### Gráfica de la direccion ip para el host PC0

![Gráfica de la direccion ip para el host PC0](lab2/pc0-ipcfg.png "Gráfica de la direccion ip para el host PC0")

### Gráfica de red privada local clase C

![Gráfica de red privada local clase C](lab2/local.png "Gráfica de red privada local clase C")

## Laboratorio 3 : Agregando segunda red local y uniendo ambas redes de forma remota

Consideremos que el segundo local tambien tiene una capaciadad para 100 equipos.

* Dirección de red (network)          : 192.168.0.128/25
* Direccion del primer miembro (host) : 192.168.0.129
* Dirección del ultimo miembro (host) : 192.168.0.254
* Dirección de difusión (broadcast)   : 192.168.0.255

### Configuración de la direción IP en el Router B
```
enable 
configure terminal
 interface Gig0/1
  ip address 192.168.0.129 255.255.255.128
  no shutdown
  exit
 exit
write mem
```

### Configuración de rutas estáticas entre las redes 192.168.0.0/25 y 192.168.1.128/25.

Para el Router A ...

```
enable
configure terminal
  ip route 192.168.0.128 255.255.255.128 10.0.0.2
  exit
write mem
```
Para el Router B ...

```
enable
configure terminal
  ip route 192.168.0.0 255.255.255.128 10.0.0.1
  exit
write mem
```

### Gráfica de dos redes privadas clase C, unidos por una red privada clase A

![Gráfica de dos redes privadas clase C](lab3/conex2locals.png "Gráfica de dos redes privadas clase C")


## Laboratorio 4 : Configurando redes VLAN en ambas sedes

Considerando las dos redes VLAN, tenemos las siguientes direcciones:

* VLAN 10: Administración, 192.168.0.0/25
* VLAN 20: Sistemas, 192.168.0.128/25

### Creando las dos redes VLANS en el Switch 0

```
enable
  configure terminal
    vlan 10
      name administracion
      exit
    vlan 20
      name sistemas
      exit
    exit
write mem
```

### Creando las dos redes VLANS en el Router A

```
enable
  vlan database
    vlan 10 name administracion
    vlan 20 name sistemas
    exit
write mem
```

### Configurando las interfaces fisicas dentro de una red VLAN en el Switch 0

```
enable 
  configure terminal
    interface Fa0/1
      switchport mode access
      switchport access vlan 10
      exit
    interface Fa0/2
      switchport mode access
      switchport access vlan 10
      exit
    interface Fa0/3
      switchport mode access
      switchport access vlan 20
      exit
    interface Fa0/4
      switchport mode access
      switchport access vlan 20
      exit
    interface Gig0/2
      switchport mode trunk
      exit
  exit
write mem
```

### Configurando las direcciones de puerta de enlace en las interfaces VLANS en el Switch 0

```
enable 
configure terminal
   interface vlan 10
     ip address 192.168.0.1 255.255.255.128
     exit
   interface vlan 20
     ip address 192.168.0.129 255.255.255.128
     exit
  exit
write mem
```

## Laboratorio 5 : Enrutamiento vector distancia y estado del enlace

