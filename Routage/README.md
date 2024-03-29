# Tp Routage

## Contexte

Schéma Logique:
![SchemaLogique.PNG](img/SchemaLogique.PNG)

Sur le logiciel de virtualisation hyperV je crée des commutateurs virtuel que pour les attribuer à mes machines:

![com.PNG](img/com.PNG)

Config hyperV de mon routeur:

![confhyperv.PNG](img/confhyperv.PNG)

 ## I. Configuration et clonage de machines Linux pour créer un serveur et des clients.
Pour configurer mes différentes interfaces j'edite le fichier /etc/network/interfaces.

Je vais sur le routeur je configure mes différentes interfaces en leurs attribuant une adresse statique:

![config3.PNG](img/config3.PNG).

Pour mettre à jour ma config je redémarre le service réseau avec la commande systemctl restart networking.

Je tape la commande ip a pour vérifier que ma config a bien été prise en compte:

![config4.PNG](img/config4.PNG)

Tables de routage des clients après les configurations:

![config5.PNG](img/config5.PNG)

![config6.PNG](img/config6.png)

je pense toujours à relancer le service réseau après chaque modification du fichier de conf des interfaces.

Test de communication:
client1 vers client2:

![config7.PNG](img/config7.PNG)

client2 vers client1:

![config8.PNG](img/config8.PNG)

## II. Configuration du routage.

Sur mon routeur :

Pour activer le routage, il faut editer le fichier /etc/sysctl.conf:
en retirant le '#' à la ligne : net.ipv4.ip_forward=1

![config1.PNG](img/config1.PNG)

Il faut ensuite relancer le service avec la commande: systemctl restart procps

La table de routage de mon routeur :

![config9.PNG](img/config9.PNG)

Nous allons utilisez un logiciel(Wireshark) permettant d’observer les trames circulant sur le routeur (ping client1 vers client2).

trame sur l'interface eth0 du routeur:

![trame1.PNG](img/trame1.PNG)

Le tableau ci-dessous (ping client1 vers client2):

![trame2.PNG](img/trame2.PNG)

trame sur l'interface eth1 du routeur:

![trame3.PNG](img/trame3.PNG)

Le tableau ci-dessous (ping client1 vers client2):

![trame4.PNG](img/trame4.PNG)

L’Address Resolution Protocol (ARP, protocole de résolution d’adresse) est un protocole utilisé pour associer l'adresse de protocole de couche réseau (typiquement une adresse IPv4) d'un hôte distant, à son adresse de protocole de couche de liaison (typiquement une adresse MAC).

Les trames sélectionnées en bleu sont des trames ARP.

## III. Routage vers l’extérieur et Internet

Je vais sur la machine de mon routeur, avec la commande ip a je vois les différentes interfaces sur ma machine.


L'interface eth0 nous permet de communiquer avec l'extérieur, pour vérifier qu'elle communique avec internet, je tape la commande ping 8.8.8.8 :

![ipa.PNG](img/ipa.PNG)


Pour que mes machines communiquent avec internet, il est nécessaire de rajouter des routes sur mon routeur.

Avant cela, il est nécesaire de configurer le fichier de conf sshd_config de mon routeur:

![ssh2.PNG](img/ssh2.PNG)

J'autorise le client, à se connecter à distance à mon routeur et je pense à redémmarer le service ssh avec la commande systemctl restart ssh.

Depuis mon client1 je vais me connecter à distance par ssh à mon routeur :

![ssh1.PNG](img/ssh1.PNG)

La connexion:

![ssh3.PNG](img/ssh3.PNG)

Je rajoute les routes:

ip route add 192.168.186.0 via 192.168.1.111

ip route add 192.168.187.0 via 192.168.1.111


Pour continuer, il faut configurer le pare feu du routeur qui sera fait dans un prochain tp.