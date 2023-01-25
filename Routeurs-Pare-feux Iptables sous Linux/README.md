# Routeurs/Pare-feux Iptables sous Linux

## I. Installation Iptables

Pour vérifier que l'on a Iptables on tape la commande Iptables -L --line-numbers :

![shell1.PNG](shell1.PNG)

Nous pouvons voir qu'aucune règles n'ont été mise en place.

Dans le cas ou la commande précédente n'est pas passée, il faudra taper la commande :

apt-get install iptables

