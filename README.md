# **Servidor DNS Maestro-Esclavo con Subdominios**

Se ha configurado un entorno con dos servidores DNS usando **BIND9**: un servidor maestro (**DNSA**) y un servidor esclavo (**DNSB**). El maestro gestiona las zonas DNS principales y permite transferencias hacia el esclavo, que actúa como respaldo. El entorno incluye configuraciones para resolver nombres en dominios y subdominios (`ies.test`, `informatica.ies.test`, `aulas.ies.test`) y consultas inversas (`192.168.57.0/24`). Además, se habilitaron logs de consultas en el servidor maestro para supervisión.

## **Archivos de Configuración**

### **DNSA (Servidor Maestro)**
1. **`/etc/bind/named.conf.local`**
   zone "ies.test" {
       type master;
       file "/etc/bind/db.ies.test";
   };
   zone "informatica.ies.test" {
       type master;
       file "/etc/bind/db.informatica.ies.test";
   };
   zone "aulas.ies.test" {
       type master;
       file "/etc/bind/db.aulas.ies.test";
   };
   zone "57.168.192.in-addr.arpa" {
       type master;
       file "/etc/bind/db.192.168.57";
   };
Archivos de Zona (/etc/bind/)
db.ies.test
db.informatica.ies.test
db.aulas.ies.test
db.192.168.57

/etc/bind/named.conf.options

options {
    directory "/var/cache/bind";
    dnssec-validation auto;
    auth-nxdomain no;
    listen-on { any; };
};

Logs (/etc/bind/named.conf)

logging {
    channel bind.log {
        file "/var/lib/bind/bind.log" versions 10 size 20m;
        severity notice;
        print-category yes;
        print-severity yes;
        print-time yes;
    };
    category queries { bind.log; };
};

/etc/resolv.conf

nameserver 127.0.0.1

DNSB (Servidor Esclavo)

/etc/bind/named.conf.local

zone "informatica.ies.test" {
    type slave;
    masters { 192.168.57.10; };
    file "/var/cache/bind/slave.db.informatica.ies.test";
};
zone "aulas.ies.test" {
    type slave;
    masters { 192.168.57.10; };
    file "/var/cache/bind/slave.db.aulas.ies.test";
};

/etc/bind/named.conf.options Igual que en DNSA.
/etc/resolv.conf

nameserver 192.168.57.10