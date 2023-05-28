# Routeur/Pare-feu Pfsense

<b>Pare-feu</b> : firewall en anglais est un dispositif de sécurité informatique conçu pour protéger un réseau informatique en contrôlant les flux de données qui y entrent et en sortent. Il agit comme une barrière de sécurité entre un réseau privé interne (comme un réseau d'entreprise ou un réseau domestique) et des réseaux externes (comme Internet) pour empêcher les accès non autorisés ou les attaques malveillantes.

Le pare-feu analyse le trafic réseau en fonction de règles prédéfinies et détermine quels paquets de données doivent être autorisés à passer et lesquels doivent être bloqués. Il peut examiner divers aspects du trafic, tels que les adresses IP sources et de destination, les ports réseau, les protocoles de communication, les types de données, etc.

<b>DMZ</b> :  est une partie d'un réseau informatique qui est spécialement configurée pour héberger des services accessibles depuis Internet tout en étant séparée du réseau interne principal. Elle agit comme une zone tampon sécurisée entre le réseau interne et le monde extérieur.

La principale raison d'utiliser une DMZ est d'isoler les services accessibles depuis Internet afin de réduire les risques potentiels pour le réseau interne. Les services couramment hébergés dans une DMZ comprennent les serveurs Web, les serveurs de messagerie, les serveurs FTP, les serveurs DNS, etc...

Schéma logique :

![img](img/f1.PNG)

## adressages 

interface em0(wan) pare-feu : 192.168.122.136

interface em1(lan) pare-feu : 192.168.1.1

interface em2(dmz) pare-feu : 192.168.2.1

webterm-1 (wan) : 192.168.122.130

webterm-2 (lan) : 192.168.2.2

PC1 (lan) : 192.168.1.3

serveur web de la dmz : 192.168.2.2

## Paramétrage Pfsense :

![img](img/f2.PNG)

![img](img/f3.PNG)

![img](img/f4.PNG)

![img](img/f5.PNG)

![img](img/f6.PNG)

![img](img/f7.PNG)

![img](img/f8.PNG)

![img](img/f9.PNG)

Il faudra ensuite redémmarer le pare-feu en selectionnant reboot.

Nous donnons une ip statique au pc1 :

![img](img/f11.PNG)

De même pour le webterm-2 :

![img](img/f12.PNG)

Via notre webterm-2 connectons nous à notre GUI pfsense :

![img](img/f13.PNG)




Nous apporterons quelques modifications :

![img](img/f14.PNG)

![img](img/f16.PNG)

Pour sécuriser notre pare-feu, nous changeons le mot de passe par défaut de pfsense par un mot de passe sécurisé (12 caratères avec des majuscules, minuscules, chiffres et caractères spéciaux) :

![img](img/f15.PNG)

Pour se connecter via un shell à notre nous activons l'authentification SSH, nous pouvons aussi changer le numéro  port ssh et mettre en système de clés pour l'authentification :

![img](img/f17.PNG)

Depuis le shell de notre webterm-2  tentant de nous connecter via ssh à notre pare-feu :

![img](img/f18.PNG)

![img](img/f19.PNG)

C'est un succès, nous pouvons vor nous 2 interfaces :

![img](img/f20.PNG)

Nous avons ajouté une nouvelle interface celle de la dmz.

![img](img/f30.PNG)

Cette commande permet d'ouvrir tous les ports sur l'interface WAN, il est important de penser à les refermer à la fin :

![img](img/f31.PNG)

Nous pouvons voir nos 3 interfaces :

![img](img/f21.PNG)

Configurons l'interface de notre dmz :

![img](img/f22.PNG)

![img](img/f23.PNG)

Dans l'onglet système puis routing, nous pouvons faire du routage :

![img](img/f24.PNG)

Nous avons hébergé un site sur notre serveur web, nous tentons d'y avoir accès depuis le lan :

![img](img/f25.PNG)

Pour illuster le fait que les adresses privé ne sont pas routables sur internet, nous mettons en place du nat, dans un cas réel l'interface coté wan est une ip publique : 

![img](img/f10.PNG)

Si un utilisateur coté wan essaye d'avoir 
accès à notre serveur web, il devra saisir l'ip de l'interface wan du pare-feu.

Pour les règles de nos différentes interfaces, nous prenons le temps de tous bloquer, puis autorisons seulement ce qui est nécessaire :

![img](img/f26.PNG)

![img](img/f27.PNG)

![img](img/f28.PNG)


Essayons depuis webterm-1 d'avoir accès au site de notre dmz :

![img](img/f32.PNG)