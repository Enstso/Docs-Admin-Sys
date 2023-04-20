# Lab02 (Vlans)

Les équipements réseau de ce lab :

- un routeur
- 3 switchs de couche 3

L'objectif est de mettre en place 3 vlans :

- vlan 10 : data
- vlan 20 : juridique
- vlan 30 : Rh


![img](img/f8.PNG) 

Nous retrouvons 2 clients par vlan reliés à des switchs différents.

Méthode configuration switch :

- Créer le vlan
- le nommer
- définir les ports du vlan 10 crée
- configurer les ports avec le mode access
- configurer le mode trunk sur le/les ports reliés à d'autres switchs

## Switchs


création des vlans sur le switch 2 :

![img](img/f1.PNG) 

![img](img/f2.PNG) 

création du lien tagué (trunk) :

![img](img/f3.PNG) 

![img](img/f10.PNG) 



## Routeur

L'encapsulation dot1q est une méthode de marquage des trames Ethernet pour permettre la transmission de plusieurs VLAN (Virtual Local Area Networks) sur un même lien physique. Elle ajoute un en-tête de 4 octets à la trame Ethernet existante, ce qui permet d'identifier le VLAN auquel appartient la trame.

Méthode configuration routeur :

- choisir des sous-interfaces
- définir l'encapsulation dot1q 
- definir des adresses ip statique qui serviront de passerelle pour nos clients

Sur notre routeur affichons les interfaces du routeur ;

![img](img/f4.PNG) 

Création des sous-interfaces :

![img](img/f5.PNG) 


J'active les interfaces du routeur avec la commande no shutdown :

![img](img/f6.PNG) 

configuration du client 1 :

![img](img/f7.PNG) 


Test :

Ping client 5 vers client 6 (vlan 30) :

![img](img/f9.PNG) 

Nous pouvons voir que le client 1 n'est pas accessible car il vient du vlan 10.




