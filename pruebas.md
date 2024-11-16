Pruebas de Funcionamiento de Consultas DNS
Este documento describe las pruebas realizadas para verificar que las consultas DNS funcionan correctamente hacia los servidores DNSA (maestro) y DNSB (esclavo) desde el ordenador anfitrión.

Configuración Previa
Archivo hosts del anfitrión

192.168.57.10 dnsa
192.168.57.20 dnsb
Archivo resolv.conf Temporal

Antes de realizar las pruebas, el archivo resolv.conf del anfitrión se configuró para apuntar a los servidores DNS:

Para probar DNSA:

nameserver 192.168.57.10
Para probar DNSB:

nameserver 192.168.57.20

Pruebas Realizadas

1. Resolución Directa de Nombres
Consulta al Maestro (DNSA): Comando:


nslookup server01.informatica.ies.test 192.168.57.10
Resultado:


Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   server01.informatica.ies.test
Address: 192.168.57.10
Consulta al Esclavo (DNSB): Comando:


nslookup server01.informatica.ies.test 192.168.57.20
Resultado:


Server:         192.168.57.20
Address:        192.168.57.20#53

Name:   server01.informatica.ies.test
Address: 192.168.57.10

2. Resolución Inversa
Consulta al Maestro (DNSA): Comando:


dig -x 192.168.57.10 @192.168.57.10
Resultado:


; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> -x 192.168.57.10 @192.168.57.10
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
10.57.168.192.in-addr.arpa. 604800 IN   PTR     server01.informatica.ies.test.
Consulta al Esclavo (DNSB): Comando:


dig -x 192.168.57.10 @192.168.57.20
Resultado:


; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> -x 192.168.57.10 @192.168.57.20
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
10.57.168.192.in-addr.arpa. 604800 IN   PTR     server01.informatica.ies.test.