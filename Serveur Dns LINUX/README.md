# Serveur Dns Linux

1 . Mise en place :
## Objectifs : Gérer un domaine et ses enregistrements. Comprendre le fonctionnement des outils de tests DNS.

Pour le serveur Dns Linux il est nécessaire d'installer les paquets bind 9 & bind 9doc.

Pour la configuration des requêtes Dns sur un serveur Dns dans le fichier   /etc/resolv.conf de mon serveur :

![resolvconf.PNG](resolvconf.PNG)

Pour les clients :

![resolvconf2.PNG](resolvconf2.PNG)
2. <u>Configuration de base pour votre zone Dns</u> :

Les fichiers de configuration DNS sont situés dans le répertoire /etc/bind. Le fichier de configuration de base est /etc/bind/named.conf.

La ligne include spécifie le nom du fichier contenant les options DNS.

![catBin.PNG](catBin.PNG)

/etc/bind/named.conf.options indique au DNS où chercher les fichiers. Tous les fichiers utilisés par BIND seront relatifs à ce répertoire.

Je vais déclarer mon réseau lan dans mon le fichier named.conf :

![namedconf2.PNG](namedconf2.PNG)

Ensuite j'accepte les requêtes pour réseau interne dans le fichier named.conf.option :

![options.PNG](options.PNG)

### configuration de ma zone personelle :

* option type master, pour inscrire une nouvelle zone principale 

* option file, pour indiquer l’emplacement du fichier de zone correspondant à mon domaine DNS. . Par défaut dans /var/cache/bind

mazone.local

Dans mon fichier de configuration de base named.conf.local :


![namedconf.PNG](namedconf.PNG)

Dans mon fichier de zone db.mazone.local :

![mazone.PNG](mazone.PNG)

j'ai ajouté un enregistrement de type A rtr qui est mon routeur et mon serveur DNS.

Après cela je redémmare le service bind9 avec la commande systemctl restart bind9

Je vais ensuite configurer la zone inverse :

![db192.PNG](db192.PNG)