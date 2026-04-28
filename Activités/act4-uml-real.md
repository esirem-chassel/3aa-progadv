# 0. Contexte

Pour un serveur de jeu, on souhaite implémenter un système de communauté en lien avec le jeu (à noter : le jeu concerné n'est PAS pertinent).

Pour faire le lien avec le serveur de jeu, on va indiquer pour chaque membre d'une communauté son identifiant en jeu.

Dans une communauté, on va également retenir son nom, une description, la date de création, le membre fondateur, et l'actuel responsable de la communauté.

Chaque membre d'une communauté peut avoir un titre informel, qui n'implique aucun rôle particulier.

Les processus d'action avec des communautés passe par le jeu en lui-même, qui effectue un appel REST authentifié avec le serveur web d'implémentation du système de communauté.

Particulièrement l'ajout d'un membre passe par une demande en jeu de l'utilisateur responsable vers le membre qu'on souhaite ajouter qui va accepter ou non directement l'invitation.
S'il accepte, alors l'appel REST est envoyé depuis le jeu vers le système de communauté.

Depuis le jeu, on peut :
- lister les communautés dont on est membre : `GET /communities`
- créer une nouvelle communauté (uniquement si l'on n'est responsable d'aucune communauté) : `PUT /communities`
- ajouter un membre à une communauté (uniquement si on en est responsable) : `PUT /communities/{0}/members`
- lister les membres d'une communauté : `GET /communities/{0}/members`
- retirer un membre d'une communauté (uniquement si on en est responsable) : `DELETE /communities/{0}/members`
- modifier le nom d'une communauté (uniquement si on en est responsable) : `POST /communities/{0}/name`
- modifier la description d'une communauté (uniquement si on en est responsable) : `POST /communities/{0}/desc`
- "donner" la communauté à quelqu'un d'autre (uniquement si on en est responsable) : `POST /communities/{0}/lead`
- supprimer la communauté (uniquement si on en est responsable) : `DELETE /communities/{0}`

Il est à noter que la suppression d'une communauté ne la supprime pas réellement, mais la place "en sursis".
Une communauté "en sursis" est réellement supprimée après un mois sans action d'édition de la part de son responsable.

Enfin, un site web permet aussi d'effectuer tous les appels de type consultation.

# 1. Diagramme de classes

Créer le diagramme de classes du système de communauté.

# 2. Diagrammes de séquence

Créer le diagramme de séquence détaillant tous les appels de gestion de communautés, particulièrement la création et l'ajout de membres.

# 3. Diagramme d'activité

Créer le diagramme d'activité détaillant l'ajout d'un membre.

# 4. Diagramme de déploiement

Proposer un diagramme de déploiement du système de communautés.
Toutes les données du sustème seront dans une base de données MongoDB.
On choisira Nodejs en tant que serveur web et serveur applicatif.
