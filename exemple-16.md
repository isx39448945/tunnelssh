# Exemple-16 Ldap-local i phpldapadmin-remot
## Engegar ldap i phpldapadmin i que tinguin connectivitat
Engegem el servidor ldap localment en un docker i configurem el /etc/hosts del mateix:

```
[munlai@localhost ~]$ docker run --rm --name ldap -h ldap -d adrinarvaez/k19:kldap
```

En una altra sessió local, configurem el fitxer /etc/ssh/sshd_config canviant la opció GatewayPorts a yes. Un cop fet això podem crear el túnel invers cap a la AMI.  

```
[munlai@localhost ~]$ ssh -i .ssh/MyKeyPair.pem -R 0.0.0.0:5000:ldap:389 fedora@3.8.102.187
```

Engegem un docker per posar en funcionament el phpldapadmin:  

```
[fedora@ip-172-31-27-185 ~]$ docker run --rm --name phpldapadmin -h phpldapadmin -it edtasixm06/phpldapadmin /bin/bash
```

Haurem de configurar el fitxer /etc/phpldapadmin/config.php afegint la IP de la màquina AMI i possar el port que hem assignat al túnel.  

També haurem de configurar el fitxer httpd afegint el ServerName.  

Una vegada fet els canvis posem en marxa el servidor web:  

```
[root@php /]# /usr/sbin/httpd
```

Una vegada hem configurat el phpldapadmin a la AMI, creem un túnel SSH directe des del nostre host local.  

```
[munlai@localhost ~]$ ssh -i .ssh/MyKeyPair.pem -L 0.0.0.0:6000:172.17.0.2:80 fedora@3.8.102.187
```

Si hem seguit tots el passos, posant a un navegador localhost:7000/phpldapadmin hauríem de ser capaços de veure'l. 

