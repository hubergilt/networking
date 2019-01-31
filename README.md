# Networking

Laboratorio 1 : Construyendo una red IPv4 ...

El diseño de una red IPv4, inicia con la selección de un rango apropiado para las direcciones IP.
A modo, de ejemplo sencillo empezamos con una red punto a punto, es decir una red de al menos dos miembros, en las distintas redes privadas es: 

Red privada | Rango Inicial | Rango Final       | Red punto a punto |
------------|---------------|-------------------|-------------------|
Clase A     | 10.0.0.0/24   | 10.255.255.255/24 | 10.0.0.0/30       |
Clase B     | 172.16.0.0/12 | 172.31.255.255/12 | 172.16.0.0/30     |
Clase C     | 192.168.0.0/8 | 192.168.255.255/8 | 192.168.0.0/30    |
