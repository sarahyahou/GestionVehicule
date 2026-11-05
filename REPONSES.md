# TP Jenkins — GestionVehicule

## Dépôt et commit du mini-défi

- Dépôt : https://github.com/sarahyahou/GestionVehicule
- Commit de main contenant le nouveau test :
  f42da4f50f54bd62b38ae9af1cbe245cffe09dc2
- Pull request du mini-défi : #7 — test/recherche-absente
- Le fichier Jenkinsfile se trouve à la racine du dépôt.

## Question 5 — Qu’est-ce qui est automatisé dans le Jenkinsfile ?
## Et qu’est-ce qui ne l’est pas ?

Le Jenkinsfile automatise :
- l’affichage du commit Git testé ;
- la compilation des classes Java ;
- le lancement des tests JUnit avec test.sh ;
- l’affichage d’un message de réussite ou d’échec.

Dans notre job « Pipeline script from SCM », Jenkins récupère aussi
automatiquement le code du dépôt configuré avant ces stages.

Le Jenkinsfile n’automatise pas l’écriture du code et des tests,
la correction des erreurs, les commits, les push ou la fusion des
pull requests. Il ne prévoit pas non plus de déploiement.

Le déclenchement automatique n’est pas défini dans ce Jenkinsfile :
il est configuré séparément dans le job Jenkins avec Poll SCM.

## Question 6 — Pourquoi récupérer le code depuis GitHub
## plutôt que depuis ~/GestionVehicule ?

Le job Jenkins est configuré pour utiliser le dépôt GitHub comme
source du code. Il récupère la branche sélectionnée dans son propre
espace de travail et teste un commit identifiable.

Le dossier ~/GestionVehicule est notre copie locale de développement.
Les modifications locales non poussées ne sont pas visibles par Jenkins.
Cela permet de tester la version partagée et enregistrée sur GitHub,
indépendamment de notre dossier de travail.

## Question 7 — Pourquoi Jenkins devient-il rouge alors que
## GitHub accepte le push ?

L’acceptation d’un push signifie que GitHub a accepté les commits
sur la branche concernée. Cela ne garantit pas que le programme
compile ni que ses tests réussissent.

Pour l’expérience, nous avons utilisé la branche isolée test/echec-ci,
sans modifier main. Le test ajoutAugmenteLeParc attendait volontairement
2 véhicules alors que le parc en contenait 1.

Le commit testé était 2bad57f.
Jenkins a trouvé 6 tests réussis et 1 test échoué.
Le script a renvoyé le code de sortie 1 et le pipeline s’est terminé
en échec.

Le hook pre-commit local a été contourné uniquement pour ce commit
volontaire, sans désactiver durablement la protection.

Après la démonstration, le job a été remis sur main, où le test correct
était conservé. Le build rouge reste une preuve dans l’historique.

## Question 8 — Le push déclenche-t-il Jenkins immédiatement
## avec Poll SCM ?

Non. Avec la planification H/5 * * * *, Jenkins interroge le dépôt
environ toutes les cinq minutes. Il lance un build lorsqu’un sondage
détecte un changement.

Nous avons ajouté une ligne au README. Comme main est protégée,
la modification est passée par la branche docs/jenkins-poll,
puis par une pull request fusionnée vers main.

La console du build automatique indique :
« Déclenché par une modification du SCM ».

Le build a exécuté les 7 tests avec succès et s’est terminé par :
« Terminé : SUCCÈS ».

## Question 9 — Mini-défi

Nouveau test ajouté :
rechercherImmatriculationAbsenteRenvoieNull

Fichier :
src/test/java/fr/nanterre/vehicules/RechercherVehiculeTest.java

Le test crée un parc contenant un véhicule, puis recherche une autre
immatriculation, absente du parc. Il vérifie que la méthode renvoie null.

Résultat local : 8 tests réussis, 0 échec.

La modification a été poussée sur test/recherche-absente, puis fusionnée
vers main par la pull request #7.

Sans cliquer sur « Build Now », nous avons observé un nouveau build
avec « Déclenché par une modification du SCM » et « Terminé : SUCCÈS ».

## Captures à joindre au rendu

1. Un build vert avec son résultat final.
2. Le build rouge de la question 7, avec l’erreur :
   attendu : <2> mais trouvé : <1>.
3. Le build automatique, avec son origine SCM et son résultat réussi.
