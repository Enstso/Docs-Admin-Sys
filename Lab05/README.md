# Lab05 Proxy Squid


Proxy : Un proxy (ou serveur mandataire) est un serveur informatique qui sert d'intermédiaire entre un client et un serveur distant. Lorsqu'un utilisateur envoie une demande à travers un proxy, celle-ci est transmise au serveur distant par le proxy au nom de l'utilisateur, et la réponse du serveur distant est renvoyée au client par le proxy.

Les proxies sont souvent utilisés pour des raisons de sécurité, de confidentialité ou de performance. Par exemple, un proxy peut être configuré pour bloquer l'accès à certains sites web ou pour filtrer le contenu de certaines pages, pour masquer l'adresse IP de l'utilisateur et protéger ainsi sa vie privée, ou pour accélérer l'accès à certains contenus en stockant une copie en cache sur le serveur proxy.

Il existe différents types de proxies, tels que les proxies HTTP, les proxies SOCKS, les proxies transparents et les proxies inversés, chacun ayant ses propres caractéristiques et utilisations.

Lab :


![img](img/f54.PNG)

## Paramétrages Lab

Nous commencons par configurer les interfaces du routeur :

![img](img/f1.PNG)

![img](img/f2.PNG)

![img](img/f3.PNG)

![img](img/f4.PNG)

![img](img/f5.PNG)

![img](img/f6.PNG)

Nous pouvons maintenant configurer l'interface du container webterm :

![img](img/f7.PNG)

Si aucun serveur Dns a été mis en place dans le fichier /etc/resolv.conf :

```sh
nameserver 8.8.8.8
```

## Configuration proxy

Pour cela il sera nécessaire d'installer les paquets suivants sur la vm :

- squid
- squidguard
- apache2-utils
- lightsquid

Il faudra ensuite créer les répertoires suivants :

![img](img/f8.PNG)


![img](img/f19.PNG)

Changement du propriétaire : 

![img](img/f9.PNG)

![img](img/f20.PNG)


Un utilisateur proxy sera le contrôleur de vos processus squid, il faut donc qu’il ait tous les droits sur les fichiers et répertoires créés ou que nous créeront. Donnez tous les droits à proxy sur les répertoires précédents.


Il faudra ensuite télécharger la blackliste :

![img](img/f10.PNG)

Donnons les droits au proxy :

![img](img/f11.PNG)

Arrêtons le proxy :

![img](img/f12.PNG)

## Configuration squid
Créeons et configurons le fichier de configuration squid :

![img](img/f13.PNG)

![img](img/f14.PNG)

![img](img/f15.PNG)

![img](img/f16.PNG)

![img](img/f17.PNG)

Générons les répertoires et fichiers de cache avec la commande suivante :

![img](img/f18.PNG)

Redémmarons le serveur squid : 

![img](img/f21.PNG)

Pour afficher le cache :

```sh
tail –f /apps/squid/log/cache.log 	
```

Pour afficher les logs d'utilisation du proxy :

```sh
tail –f   /apps/squid/log/access.log
```
Testons, configurons notre navigateur sur notre container :

![img](img/f22.PNG)

Nous pouvons dans le fichier /apps/squid/log/access.log que la machine 192.168.186.2 (container) est bien passé par le proxy :

![img](img/f23.PNG)

## Configuration squidGuard

Nous allons maintenant améliorer la sécurité de squid en lui ajoutant un logiciel qui va permettre l’interdiction de site Web en fonction de blacklists. Il permet bien d’autres options mais nous nous concentrerons sur celles ci. L’utilisation des blacklists nécessite une observation préalable de l’arborescence existante pour chaque groupe d’interdiction.

Renommons le fichier /etc/squidguard/squidGuard.conf, afin de faire le nôtre :

![img](img/f24.PNG)


ACL: Un ACL Proxy, ou "Access Control List Proxy" en anglais, est une technique utilisée pour limiter l'accès à un réseau ou à Internet. Cela implique l'utilisation d'un serveur proxy pour filtrer et bloquer les requêtes entrantes ou sortantes en fonction de critères prédéfinis.

L'ACL Proxy utilise une liste de contrôle d'accès pour déterminer si une demande doit être autorisée ou refusée. Cette liste de contrôle d'accès peut être basée sur l'adresse IP de l'utilisateur, le type de demande (par exemple, HTTP ou FTP), le port de destination, le contenu de la demande ou tout autre critère approprié.

L'utilisation d'un ACL Proxy peut aider à renforcer la sécurité du réseau en empêchant les utilisateurs non autorisés d'accéder à des ressources spécifiques, en bloquant les attaques potentielles et en limitant l'exposition du réseau aux menaces externes. Cependant, il est important de noter que l'ACL Proxy n'est pas une mesure de sécurité à elle seule, et doit être utilisé en conjonction avec d'autres techniques de sécurité pour une protection optimale.

Vérifions avant de créer notre acl que la liste est bien présente :

![img](img/f25.PNG)

Nous pouvons créer notre fichier de configuration /etc/squidguard/squidGuard.conf :

![img](img/f26.PNG)

![img](img/f27.PNG)

Dans le fichier /etc/squid/squid.conf nous spécifions le démarrage de squidguard :

![img](img/f28.PNG)

Relancons squid et indexons la db de squidguard :

![img](img/f29.PNG)

Essayons d'accéder à un site de jeux d'argent en ligne :

![img](img/f30.PNG)

En https :

![img](img/f31.PNG)

Youtube est un site, que nous n'avons bloqué :

![img](img/f32.PNG)


Si dans le fichier /apps/squid/log/access.log vous remarqué des messages avec le mot HIER_none, il sera nécéssaire de rajouter la ligne suivante dans /etc/squid/squid.conf :

![img](img/f33.PNG)

## Plus de contraintes sous squid et squidGuard

### Plage horaires :

Créeons notre propre liste :

![img](img/f37.PNG)

Nouvelle destination :

![img](img/f38.PNG)

Définition de la plage horaire (vendredi entre 21:45 et 22:00) :

![img](img/f39.PNG)

Redéfinition de notre acl :

![img](img/f40.PNG)

Testons, il est 21:45 rechargons notre page :

![img](img/f34.PNG)

Nous pouvons voir que youtube n'est plus accessible :

![img](img/f35.PNG)

C'est un succès youtube est de nouveau accessiblecar il est 22h02 :


![img](img/f36.PNG)

### Authentification :

Créeons un fichier qui contiendra le nom d'utilisateur et le mot de passe permettant l'authentification :

![img](img/f41.PNG)

Donnons la propriété à proxy.

Cette commande permet de spécifier que le username est root et le mot de passe est siojjr cela sera inscrit dans le fichier users :

![img](img/f42.PNG)

Dans le fichier /etc/squidguard/squidguard.conf :

![img](img/f43.PNG)



![img](img/f44.PNG)

![img](img/f45.PNG)

Testons, sur notre container :

![img](img/f46.PNG)

Si nous refusons de nous authentifier, il sera impossible d'avoir accès à internet depuis ce navigateur :

![img](img/f47.PNG)

Sur une machine virtuelle :

![img](img/f48.PNG)

C'est un succès, nous pouvons naviguer sur internet :

![img](img/f49.PNG)

