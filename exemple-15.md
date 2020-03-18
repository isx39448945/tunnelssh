# Exemple-15 Ldap-remot i phpldapadmin-local  
## Desplegar el servei ldap  
Si volem fer que el servidor ldap sigui remot y es posi en funcionament haurem de configurar un host-remot AMI AWS EC2.  
En aquest host farem ús d'un security groups que només obrirà el port 22 per l'SSH ja que el de ldap (389) no farà falta gràcies al túnel.  
També afegirem una línia al /etc/hosts de la AMI fent referència al container ldap.  

Posem el host remot en marxa:  

```
[munlai@localhost ~]$ ssh -i .ssh/MyKeyPair.pem fedora@18.130.211.6
[fedora@ip-172-31-27-185 ~]$ docker run --rm --name ldap -h ldap -d adrinarvaez/k19:kldap
```

Creem un túnel SSH directe al servidor ldap de la AMI:

```
[munlai@localhost ~]$ ssh -i .ssh/MyKeyPair.pem -L 0.0.0.0:5000:ldap:389 fedora@18.130.211.6
```

Ara toca la part del phpldapadmin. Creem el docker:

```
[munlai@localhost ~]$ docker run --name phpldapadmin -h phpldapadmin -it edtasixm06/phpldapadmin /bin/bash
```

Haurem de configurar el fitxer /etc/phpldapadmin/config.php afegint la IP del host local i possar el port que hem assignat al túnel.  

Una vegada fet els canvis posem en marxa el servidor web:  

```
systemctl start httpd
```

Si hem seguit tots el passos, posant a un navegador la ip del docker (172.17.0.2/phpldapadmin) hauríem de ser capaços de veure'l.
