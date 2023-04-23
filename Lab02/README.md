# Lab02 (Vlans)

Vlan : Un VLAN (Virtual Local Area Network) est un réseau local virtuel qui permet de regrouper des périphériques réseau en fonction de critères tels que la fonction, le service, le département ou l'emplacement géographique, indépendamment de leur emplacement physique dans le réseau.

En d'autres termes, il s'agit d'une méthode de segmentation logique d'un réseau physique en plusieurs sous-réseaux distincts, qui peuvent communiquer entre eux selon les besoins. Les VLAN sont souvent utilisés pour améliorer la sécurité et la gestion de réseau, ainsi que pour optimiser les performances en réduisant la taille des domaines de diffusion.

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




## VTP

VTP signifie "VLAN Trunking Protocol". C'est un protocole de communication utilisé dans les réseaux informatiques pour faciliter la gestion des réseaux VLAN (Virtual LAN).

VTP permet aux commutateurs de communiquer les informations de configuration des VLANs entre eux de manière automatique et cohérente, sans que les administrateurs réseau aient à configurer manuellement chaque commutateur.


Pour le mettre en place nous ajouterons un switch supplémentaire, de plus nous mettrons un switch en mode transparent.

- sw1 : mode server
- sw2 : mode transparent
- sw3 : mode client
- sw4 : mode client


Sur notre switch nous pouvons que le mode serveur est activé :

![img](img/f11.PNG)

Nous créeons notre domaine de vlans :

![img](img/f12.PNG)

Nous ajoutons un nouveau switch (sw4) :

![img](img/f17.PNG)

Par défaut il est en mode serveur :

![img](img/f13.PNG)

Nous le mettons en mode client :

![img](img/f14.PNG)

Le switch 3 est relié au switch 4 il est nécessaire de configurer le mode trunk :

![img](img/f16.PNG)

Créeons le vlan 40 imprimante sur notre switch 1 qui a le mode server : 

![img](img/f18.PNG)

Sur les switchs 3 et 4  nous pouvons voir que le vlan 40 est bien présent :

![img](img/f19.PNG)

![img](img/f20.PNG)

Essayons de créer un vlan sur notre switch 4 :

![img](img/f21.PNG)

Cela ne marche car il possède le mode client.



Nous configurons notre switch 2 en mode transparent :

![img](img/f15.PNG)

Nous créeons un nouveau vlan 50 (dev) :

![img](img/f22.PNG)

Le vlan a bien été enregistré.



