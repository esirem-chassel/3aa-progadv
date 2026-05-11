
# Concept

## Objectif global

Dans cette copie éhontée d'un certain jeu fortement streamé durant un temps, le joueur se retrouve face à un NPC, avec une arme chargée au centre.
A chaque manche, l'arme est chargée d'un certain nombre de balles à blanc et de balles réelles.
Le joueur doit prendre des décisions, en s'aidant de jokers, pour forcer l'adversaire à se tirer dessus.

Chaque participant commence avec 5 points de vie.
Les manches s'enchaînent jusqu'à ce qu'un joueur tombe à court de points de vie.
Une manche se termine lorsqu'il n'y a plus de balles dans l'arme.

## Choix du tir

A chaque tour, chaque participant va décider s'il se tire dessus ou s'il tire sur l'adversaire.

## Déroulé d'une manche

A chaque manche :
- on indique le nombre de balles "à blanc" et le nombre de balles "réelles" (désignées aléatoirement, il y en a toujours au moins une de chaque, maximum total de 6)
- on distribue 2 jokers aléatoires à chaque participant
- on tire à pile ou face pour savoir qui commence (le gagnant désigne qui commence)

## Déroulé d'un tour

A chaque tour, le joueur peut :
- utiliser autant de jokers qu'il le désire, s'il en a en stock
- soit tirer sur soi-même, soit tirer sur l'adversaire

# Consignes

Toute l'application est à réaliser en Java en ligne de commande. Aucune interface n'est nécessaire; pour le moment.

Sont attendus :
- le diagramme UML de classes du système
- un diagramme objet de l'état d'initialisation du système
- le code d'ensemble, en Java du système

# Annexes

## Liste des jokers

Jokers :
- voir la prochaine balle
- soin d'une unité de vie
- obtenir 2 nouveaux jokers
- inverser la prochaine balle
- la prochaine balle inflige 2x de dégats
- voir au hasard une balle quelque part (et sa position)
- effectuer une action supplémentaire (se tirer dessus ou sur l'autre)
