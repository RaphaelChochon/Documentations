## Installation de Weewx sur Raspberry Pi sous Raspbian Jessie (8)
###### Dernière MAJ du mémo : 04/06/2016


### 0 - Divers liens utiles


* [Site officiel Weewx](http://www.weewx.com/)
* [Docs officielles Weewx](http://www.weewx.com/docs.html)
* [GitHub Weewx](https://github.com/weewx/weewx)
* [Wiki Weewx](https://github.com/weewx/weewx/wiki)
* [Wiki Weewx & RPI](https://github.com/weewx/weewx/wiki/Raspberry%20Pi)
* [Blog driver WMR200](http://iadetout.free.fr/?wview-sur-Ubuntu-12-04) : Conseils ferrite et driver Oregon WMR200 sur RPI et ``Wview`` (applicable a ``Weewx``)


### 1 - Pré-requis WMR200


Pour le driver WMR 100 (WMR88) à priori pas besoin d’effectuer cette manip, mais cela reste à confirmer...
<br>
* Le driver USB : Il ne faut pas utiliser usbhid

Éditer le fichier ``blacklist-wmr200.conf`` de cette manière :

```
	sudo nano /etc/modprobe.d/blacklist-wmr200.conf
```

Et y ajouter :

```
	options usbhid quirks=0x0fde:0xca01:0x4
```

Ensuite :

```
	sudo nano /etc/rc.local
```

Et y ajouter :

```
	modprobe -r usbhid
	modprobe  usbhid
```

Enfin controller avec ``lsusb`` :

```
	$ lsusb
	...
	Bus 001 Device 032: ID 0fde:ca01
	Ou plutôt : Bus 001 Device 004: ID 0fde:ca01
```


### 2 - Installation de différents paquets et dépendances


Weewx en aura besoin (méthode setup.py install)

```
	sudo apt-get install python-configobj python-cheetah python-cheetah
	sudo apt-get install python-usb
	sudo apt-get install mysql-client python-mysqldb
	sudo apt-get install ftp
	sudo apt-get install python-dev python-pip
	sudo pip install pyephem
```


### 2 - Téléchargement et installation de Weewx


:bangbang: **Vérifier d'abord le numéro de version de la dernière version de Weewx [sur le site](http://www.weewx.com/news.html)**<br>
Au 04/06/2016 : 3.5.0

```
	cd /home/pi
	sudo wget http://weewx.com/downloads/weewx-x.x.x.tar.gz
```

On décompresse l'archive et on installe :

```
	sudo tar xvfz weewx-3.5.0.tar.gz
	cd weewx-x.x.x
	./setup.py build
	sudo ./setup.py install
```

A ce moment, on pourra répondre aux différentes questions directement dans la console ce qui permettra de commencer à remplir le fichier de conf.


### 3 - Édition du fichier de conf


Le fichier de configuration globale de Weewx se trouve dans ``/home/weewx/weewx.conf``.
On peut l'éditer avec nano, ou bien avec une connexion en sftp sur un autre ordinateur, ce sera plus pratique.
Emplacement du fichier de conf par défaut pour l'association Nice Météo 06 : https://github.com/RaphaelChochon/Weewx-RPI


### 4 - Activation du daemon


Activer le daemon permet en cas de reboot de la RPI, un redémarrage automatique de Weewx. Pour cela :

```
	cd /home/weewx
	sudo cp util/init.d/weewx.debian /etc/init.d/weewx
	sudo chmod +x /etc/init.d/weewx
	sudo update-rc.d weewx defaults 98
```

Et enfin pour démarrer une bonne fois pour toute Weewx :

```
	sudo /etc/init.d/weewx start
```


### 5 - Pour désinstaller Weewx


```
	sudo rm -r /home/weewx
	sudo rm /etc/init.d/weewx
```


### 6 - Quelques commandes utiles


Pour démarrer, stopper ou redémarrer le daemon de Weewx :

```
	sudo /etc/init.d/weewx start
	sudo /etc/init.d/weewx stop
	sudo /etc/init.d/weewx restart
```

***

Pour recharger la configuration (en cas de modification d'un fichier ``.conf``)

```
	sudo service weewx reload
```

***

Pour consulter le log en direct

```
	sudo tail -f /var/log/syslog
```

***

Pour générer un rapport immédiatement sans attendre le suivant (utile pour tester les thèmes (skins))

```
	./bin/wee_reports
```


### 7 - Utilisation de l'utilitaire wee_config de Weewx


Emplacement de l'utilitaire :

```
	/home/weewx/bin/wee_config
```

Cela permet par exemple de reconfigurer le driver :

```
	/home/weewx/bin/wee_config --reconfigure --driver=weewx.drivers.vantage
```

Plus de détails dans [cette partie de la doc de Weewx](http://www.weewx.com/docs/usersguide.htm#wee_config_utility)


### 8 - BUG

Ce n'est pas un bug de Weewx, mais d'une dépendance de Raspbian Jessie (en fait ce n'est pas vraiment un bug apparemment...)<br>
Dans le syslog le message suivant apparait à interval très (trop) régulier :

```
	System rsyslogd-2007: action 'action 17' suspended, next retry is Wed Feb  4 16:39:31 2015 [try http://www.rsyslog.com/e/2007 ]
```

Pour résoudre ce problème, il suffit de commenter dans le fichier de conf de ``Rsyslog`` les 4 dernières lignes comme ci dessous :

```
	sudo nano /etc/rsyslog.conf
```

```
	#daemon.*;mail.*;\
	#   news.err;\
	#   *.=debug;*.=info;\
	#   *.=notice;*.=warn
```

Et enfin effectuer un reboot :

```
	sudo reboot
```


### 9 - Rappel de l'emplacement des différents répertoires de Weewx


Type | Emplacement
------------ | -------------
Executable | /home/weewx/bin/weewxd
Fichier de conf | /home/weewx/weewx.conf
Utilitaires | /home/weewx/bin/
Thèmes et templates | /home/weewx/skins/
BDD SQLite | /home/weewx/archive/
Pages web et graphs générés | /home/weewx/public_html/
Documentation | /home/weewx/docs/





