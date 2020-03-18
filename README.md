# Pràctica túnels SSH "The Crazy Thing"
## Exemple-15 Ldap-remot i phpldapadmin-local  

Desplegem dins d’un container Docker (host-remot) en una AMI (host-destí) el servei ldap amb el firewall de la AMI només obrint el port 22. Localment al host de l’aula (host-local) desplegem un container amb phpldapadmin. Aquest container ha de poder accedir a les dades ldap. des del host de l’aula volem poder visualitzar el phpldapadmin.  

### Desplegar el servei ldap 

- en el host-remot AMI AWS EC2 engegar un container ldap sense fer map dels ports.  
- en la ami cal obrir únicament el port 22
- també cal configurar el /etc/hosts de la AMI per poder accedir al container ldap per nom de host (preferentment).  
- verificar que des del host de l’aula (host-local) podem fer consultes ldap.  

### Desplegar el servei phpldapadmin  

- engegar en el host de l’aula (host-local) un container docker amb el servei phpldapadmin fent map del seu port 8080 al host-local (o no).  
- crear el túnel directe ssh des del host de l’aula (host-local) al servei ldap (host-remot) connectant via SSH al host AMI (host-destí).  
- configurar el phpldapadmin per que trobi la base de dades ldap accedint al host de l’aula al port acabat de crear amb el túnel directe ssh.
- ara ja podem visualitzar des del host de l’aula el servei phpldapadmin, accedint al port 8080 del container phpldapadmin o al port que hem fet map del host de l’aula (si és que ho hem fet).  

## Exemple-16 Ldap-local i phpldapadmin-remot  

Obrir localment un ldap al host. Engegar al AWS un container phpldapadmin que usa el ldap del host d el’aula. Visualitzar localment al host de l’aula el phpldapadmin del container de AWS EC2.  

### Engegar ldap i phpldapadmin i que tinguin connectivitat  

- engegar localment el servei ldap al host-local de l’aula.  
- obrir un túnel invers SSH en la AMI de AWS EC2 (host-destí) lligat al servei ldap del host-local de l’aula.  
- engegar el servei phpldapadmin en un container Docker dins de la màquina AMI. cal confiurar-lo perquè connecti al servidor ldap indicant-li la ip de la AMI i el port obert per el túnel SSH.  
- atenció al binding que fa ssh dels ports dels túnels SSH (per defecte són només al localhost).  

### Ara cal accedir des del host de l’aula al port 8080 del phpldapadmin per visualitzar-lo. Per fer-ho cal:  

- en la AMI configutat el /etc/hosts per poder accedir per nom de host (per exemple php) al port apropiat del servei phpldapadmin.  
- establir un túnel directe del host de l’aula (host-local) al host-remot phpldapadmin passant pel host-destí (la AMI).  
- ara amb un navegador ja podem visualitzar localment des del host de l’aula el phpldapadmin connectant al pot directe acabat de crear.  
- atenció al binding que fa ssh dels ports dels túnels SSH (per defecte són només al localhost).  


