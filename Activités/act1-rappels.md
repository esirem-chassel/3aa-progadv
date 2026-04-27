# 0. Contexte

On souhaite gérer une liste d'étudiants, avec leurs absences.
Pour cela, on va créer une structure représentant un étudiant avec nom, prénom, et le nombre de demi-journées d'absences.
Pour simplifier l'application, on ne prendra pas en compte les accents.
Toute l'application sera en console via langage C (et non C++).

# 1. Création du menu principal

Créez le menu principal d'application, qui affichera quelque chose comme :
```
[N]ouvel etudiant
[L]iste des etudiants
[A]bsence
[Q]uitter
? 
```

Et permettra:
- si N est saisi, de saisir un nouvel étudiant (à venir : `void ajoutEtudiant()` )
- si L est saisi, de lister les étudiants avec nom, prénom, et le nombre de demi-journées d'absences (à venir : `void listeEtudiants()` )
- si A est saisi, de chercher un étudiant par nom, pour ajouter une absence (à venir : `void ajoutAbsence()` )
- si Q est saisi, de quitter l'application

Une fois l'action effectuée, on reviendra au menu principal.

> [!TIP]
> Créez une (ou plusieurs ?) fonctions pour cela !

# 2. Création de la structure Etudiant et fonction afficherEtudiant

Créez une structure (nommée) Etudiant, en respectant les bonnes règles de nommage vues jusqu'ici.
Quel est son poids ?

Pour le test, lors du lancement de l'application, créez programmatiquement deux étudiants "bidons", et testez leur affichage, via une fonction dédiée `afficherEtudiant`.

# 3. Liste d'étudiants

Nous devons maintenant créer une liste d'étudiants.
Comment feriez-vous avec un tableau ? Quelle est la limite inhérente à C ?

Créez une liste chaînée pour conserver une liste d'étudiants. Testez votre liste chaînée programmatiquement.

# 4. Saisie d'étudiants

Complétez la fonction `void ajoutEtudiant()` qui permettra de saisir un nouvel Etudiant.
Aucun contrôle de nom n'est réalisé.

# 5. Listing d'étudiants

Complétez la fonction `void listeEtudiants()` qui permettra de lister les Etudiants.
La liste sera sortie dans l'ordre "tel quel" (sans ordre particulier).

# 6. Saisie d'absences

Complétez la fonction `void ajoutAbsence()` qui permet de recherche un étudiant par son nom de famille en vue d'ajouter une absence.
On ne gère pour le moment PAS les homonymes.

# 7. Gestion des homonymes

Que se passe-t-il si on souhaite saisir une absence et que deux étudiants ont le même nom de famille ?
Comment résoudriez-vous cela ?
