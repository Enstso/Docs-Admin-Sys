# Routeurs/Pare-feux Iptables sous Linux

## I. Installation Iptables

Pour vérifier que l'on a Iptables on tape la commande Iptables -L --line-numbers :

![shell1.PNG](shell1.PNG)

Nous pouvons voir qu'aucune règles n'ont été mise en place.

Dans le cas ou la commande précédente n'est pas passée, il faudra taper la commande :

apt-get install iptables

## II. Mise en oeuvre de netfilter

### a) Script de base
Pour éviter les erreurs je vais créer les différents règles du pare-feux dans script sh.

le script permettant de revenir à la politique de base + nat pour que les autres puissent communiquer avec internet(parefeu_off) :

![img1.PNG](img1.PNG)

le script permettant de changer la politique par défault c'est celui dan slequel je vais travailler(parefeu_on) : 

![img2.PNG](img2.PNG)

Après ça je fais un chmod +x nom du script 

pour qu'il soit éxécutable.

pour lancer le script je tape la commande ./parefeu_on

je tape la commande iptables -L --line-numbers pour vérifier que tout soit bien refusée :

![img3.PNG](img3.PNG)

### b) Filtrage simple

Dans le fichier parefeu_on nous ajouterons et effecerons des règles au fur et à mesure.

Sur le machine parefeu nous allons configurer les flux ssh :

installer ssh : apt-get install openssh-server

