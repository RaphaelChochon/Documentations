## Installation et configuration de Geoserver 2.8.3
###### Dernière modif tuto : 11/04/2016
###### Environnement : Debian 8


#### 1 - Pré-requis
###### 1.1 - Installation de l’environnement Java 7 (JRE)

```
	apt-get update
	apt-get install openjdk-7-jre
```

###### 1.2 - Installation de Tomcat7

```
	apt-get install tomcat7 tomcat7-admin
```

###### 1.3 - Création user Tomcat

On créé un user de connexion pour le manager web de Tomcat
Pour cela, édition du fichier ``tomcat-users.xml`` de cette manière : ``sudo nano /etc/tomcat7/tomcat-users.xml``

```
	<role rolename="manager-gui"/>
	<user username="utilpnm" password="MON_MOT_DE_PASSE" roles="manager-gui"/>
```


#### 2 - Installation de Geoserver
