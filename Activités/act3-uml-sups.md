# 0. Contexte

L'objectif de cette activité est de travailler davantage UML au-delà du diagramme de classes.
Pour cela on va se mettre dans la situation suivante :
- nous devons déployer un applicatif complet sur un serveur vierge
- le serveur est une RHEL fraîchement clonée d'une distribution de 2020
- l'application est composée
  - d'un frontend déployé par CI/CD de type React
  - d'un backend déployé manuellement basé sur PHP
- un serveur web lighthttpd gérera la partie web
- un reverse proxy nginx qui servira également de cache, intercalé avant le serveur web


# 1. Déploiement

Dressez le diagramme de déployement de ce système.

# 2. Composants

Dressez le diagramme de composants de ce système.

# 3. OAuth (activité et séquence)

## 3.1 OAuth
OAuth est un protocole purement applicatif permettant la connexion à travers un processus externe, pour permettre par exemple du SSO.
On va distinguer, pour simplifier, trois parties dans le processus :
- l'application, la nôtre
- l'utilisateur
- le service d'authentification (par exemple, Google, dans le cadre d'une connexion via un compte Google)

Lorsqu'une demande de connexion OAuth est effectuée,
l'application envoie au serveur d'authentification une demande en fournissant:
- une URI de retour
- un scope, définissant l'ensemble des permissions recherchées
L'application obtient donc une URI de connexion (contenant certains paramètres), vers laquelle elle dirige alors l'utilisateur.
L'utilisateur s'authentifie donc auprès du serveur d'authentification via cette URI, qui renvoie vers l'URI de retour initialement spécifiée avec les données demandées,
si la connexion est valide.

Seuls des token plus ou moins temporaires sont donc transmis à notre application, et l'authentification se fait bien uniquement auprès du site d'authentification.

## 3.2 Séquence

Dresser le diagramme de séquence d'OAuth tel qu'on l'a expliqué en 3.1

## 3.3 Activité

Dresser le diagramme d'activité d'OAuth (coté serveur applicatif) tel qu'on l'a expliqué en 3.1
