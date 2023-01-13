# Serveur Dns Linux

## Objectifs : Gérer un domaine et ses enregistrements. Comprendre le fonctionnement des outils de tests DNS.

1 .  <u>Mise en place</u> :

Pour le serveur Dns Linux il est nécessaire d'installer les paquets bind 9 & bind 9doc.


2 . <u>Configuration de base pour votre zone Dns</u> :

Les fichiers de configuration DNS sont situés dans le répertoire /etc/bind. Le fichier de configuration de base est /etc/bind/named.conf.

La ligne include spécifie le nom du fichier contenant les options DNS.

![catBin.PNG](catBin.PNG)

/etc/bind/named.conf.options indique au DNS où chercher les fichiers. Tous les fichiers utilisés par BIND seront relatifs à ce répertoire.

Dans le fichier named.conf est spécifié où le serveur Dns doit récuperer les zones /etc/bind/named.conf.local :

![namedconf2.PNG](namedconf2.PNG)

Pour éviter tous problèmes avec l'ipv6 dans  le fichier named.conf.option à listen-on-v6 j'ajoute entre les accolades ::1; :

![options.PNG](options.PNG)

### configuration de ma zone personelle :

* option type master, pour inscrire une nouvelle zone principale 

* option file, pour indiquer l’emplacement du fichier de zone correspondant à mon domaine DNS. . Par défaut dans /var/cache/bind

### mazone.local

Dans mon fichier de configuration de base named.conf.local je définis mes zones :


![namedconf.PNG](namedconf.PNG)

Dans mon fichier de zone db.mazone.local :

![mazone.PNG](mazone.PNG)

j'ai ajouté des enregistrements de type A, ex : rtr qui est mon routeur et mon serveur DNS.

Après cela je redémmare le service bind9 avec la commande systemctl restart bind9.

Je vais ensuite configurer la zone inverse :

![db192.PNG](db192.PNG)

Pour la configuration des requêtes Dns sur un serveur Dns dans le fichier   /etc/resolv.conf de mon serveur :

![resolvconf.PNG](resolvconf.PNG)

Pour les clients :

![resolvconf2.PNG](resolvconf2.PNG)

Pour que mes configurations soient prises en compte je relance service bind9 :

systemctl restart bind9

Test ping nom de machine du client 1 depuis le routeur(serveurDns) :

![ping1.PNG](ping1.PNG)

ping client 1 vers client2 :

![ping2.PNG](ping2.PNG)


nslookup est un outil en ligne de commande simple mais très pratique qui est principalement utilisé pour trouver l’adresse IP d’un hôte spécifique ou le nom de domaine d’une adresse IP spécifique (recherche DNS inverse).

Nous pouvons aussi verifier son fonctionnement en testant les enregistrements avec nslookup :

rtr :

![nslookup1.PNG](nslookup1.PNG)

client 2 :

![nslookup2.PNG](nslookup2.PNG)


Par défaut, mes machines lorsqu'elles ne connaissent pas un nom de domaine. La requête sera renvoyé vers le serveur dns de ma machine local qui transmettra cette requête à mon Fai si il ne l'a pas en cache.

Pour faire évoluer, mon serveur DNS je lui rajouterais une zone secondaire.

