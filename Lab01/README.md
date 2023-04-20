# Lab01

Les équipements réseau de ce lab :

- un routeur
- 2 switchs de couche 3


Pour compléter cela j'ajoute 4 clients, 2 pour chaque switchs.

![img](img/f1.PNG) 

Configuration du routeur 1 :

![img](img/f4.PNG) 

je tape write pour enregistrer ma configuration :

![img](img/f5.PNG) 

Vérifions la configuration :

![img](img/f6.PNG) 

Configuration du switch 1 :

![img](img/f3.PNG) 

Pour mon client je lui spécifie sa passerelle 192.168.186.1.

Configuration du client 1 :

![img](img/f2.PNG) 

Test

ping client 1 vers client 2 :

![img](img/f7.PNG) 


ping client 3  vers client 4 :

![img](img/f8.PNG) 


ping client 1 vers client 4 :

![img](img/f9.PNG) 

Le routage est géré par défaut c'est la raison pour laquelle les deux réseaux arrivent à communiquer.