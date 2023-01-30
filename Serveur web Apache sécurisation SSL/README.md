# Serveur Web Apache Sécurisation via SSL.

 Un serveur web est juste un ordinateur qui stocke, traite et livre des fichiers de site web aux navigateurs web.

Les serveurs web sont constitués de matériel et de logiciels qui utilisent le protocole de transfert hypertexte (HTTP) pour répondre aux demandes des internautes faites via Internet.(https://www.hostinger.fr/tutoriels/serveur-web)

Le port 80 est le numéro de port attribué au protocole de communication Internet couramment utilisé, Hypertext Transfer Protocol (HTTP). Il s'agit du port à partir duquel un ordinateur envoie et reçoit des communications et des messages basés sur un client Web d'un serveur Web et est utilisé pour envoyer et recevoir des pages ou des données HTML.(https://fr.theastrologypage.com/port-80)

Le port 443 est un port utilisé pour les communications sécurisées sur Internet. Il est principalement utilisé pour établir des connexions HTTPS, qui sont des connexions chiffrées basées sur le protocole HTTP. Le port 443 est crucial pour protéger les données privées des utilisateurs lorsqu’ils naviguent sur Internet, en évitant les attaques de type “man-in-the-middle” et les sites web frauduleux.(https://tt-hardware.com/pc/port-443/)

Schéma logique :

![lan.PNG](lan.PNG)
### Objectifs : 	

- Configurer une machine Linux en réseau
- installer un serveur LAMP
- Mettre en œuvre un site Web
- Sécurisation avec un certificat pour le standard HTTPS.

## Configuration du serveur web :

enregistrement dns :

![dns.PNG](dns.PNG)


![dns2.PNG](dns2.PNG)

enregistrement dns inverse :

![dns1.PNG](dns1.PNG)

réservation de l'adresse ip auprès du serveur Dhcp :

![dhcp.PNG](dhcp.PNG)

Pour que le serveur web  installe des paquets il faut autoriser les ports ftp :

![ftp.PNG](ftp.PNG)

Si un serveur Dns, n'a pas été mis en place il faudra éditer le fichier hosts de toutes les machines, ce qui est long est embétant si on a une centaine de machine dans le réseau.

exemple :
Pour la résolution du nom du serveur web nous pouvons éditer le fichier /etc/host et ajouter cette ligne :

![host.PNG](host.PNG)

## Installation et configuration de base d'apache2 :

il faut installer différents paquets apache2, apache2-utils, mariadb-server, php7.4, php7.4-mysql

Pour télécharger php7.4 :

```sh
sudo apt -y install lsb-release apt-transport-https ca-certificates
``` 

```sh
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
```
```sh
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
```
```sh
 apt-get update
```
```sh
apt -y install php7.4
```

Je crée un répertoire dans /var/www/ :

```sh
mkdir /var/www/monsite
```

je crée deux pages Web une en html, l’autre en PHP :

![ls.PNG](ls.PNG)

Ensuite je crée le fichier de configuration de mon site :

```sh
touch /etc/apache2/sites-available/monsite.conf
```
Je configure, mon virtual host dans mon fichier de conf :

![vh.PNG](vh.PNG)

``` sh
ServerName monsite 	# FQDN permettant l’accès au site
DocumentRoot /var/www/monsite # répertoire de base des pages HTML
<Directory /var/www/monsite> #droit d’accès au pages donc au site !
	Require all granted	#Autorisation pour tout le monde
</Directory>
```
Pour activer le site il faut taper la commande :

```sh
a2ensite monsite.conf
```

cette commande permet d'active un site qui est lié à un fichier conf qui contient un virtual host et cela créer un lien symbolique dans  /etc/apache2/sites-enabled/

Pour désactiver le site il faut taper la commande :

```sh
a2dissite monsite.conf
```

On redémmare le service apache2.

Test depuis mon routeur :

![site.PNG](site.PNG)