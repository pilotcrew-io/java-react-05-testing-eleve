# Évaluation — jr-05-testing · 20 points

| Critère | Points | Preuve attendue |
|---|---:|---|
| Matrice de risques et choix de niveaux | 3 | Chaque risque possède une preuve nommée et une limite. |
| Tests Java isolés et discriminants | 4 | Montant, idempotence, événement et HTTP ; horloge et mémoire contrôlées. |
| Tests React centrés sur le comportement | 4 | PENDING, erreur, clé stable et nettoyage ; mocks conformes au contrat. |
| Parcours navigateur avec vraie API locale | 4 | Rapport Playwright, commande créée, événement signé et résultat affiché. |
| Deux mutations rouge puis vert | 3 | Une régression Java et une React détectées par assertions pertinentes. |
| Automatisation, rapports et revue honnête | 2 | Script d’exécution, code de sortie, commit et statut réel des preuves. |
| **Total** | **20** | |
## Règles de restitution

Un fichier présent n’obtient pas à lui seul tous les points : montrer son comportement et pouvoir l’expliquer. Les cas négatifs sont aussi importants que le nominal. Le code rendu ne contient aucune vraie donnée bancaire ou identité PSP ; tous les scénarios restent locaux et fictifs.

Associer chaque preuve à la version remise. Le journal mentionne l’acquis repris du lab précédent et toute base fournie par l’encadrant. Une revue locale est recevable lorsqu’aucun dépôt distant n’est utilisé. Ne pas créer de fausse URL de commit, de pull request ou de rapport.

En cas de panne d’outillage documentée, l’encadrant distingue les critères vérifiés sur un poste de secours, les contrôles relus mais non exécutés et les preuves reportées. Un test navigateur non exécuté ne reçoit pas l’étiquette « E2E réussi ». Une reprise de preuve peut être organisée ; le résultat obtenu reste daté et attribué au bon environnement.

Pour valider la séance, le formateur dispose d’un ensemble fonctionnel sur les objectifs obligatoires, de tests pertinents, d’un diagnostic expliqué et de limites honnêtes. Une extension ne compense pas un montant client accepté, une duplication masquée ou un faux succès affiché.

## Questions d’oral

1. Nommer la frontière simulée dans chaque test.
2. Expliquer le lien entre risque et assertion.
3. Dire ce qu’un test E2E local ne prouve pas sur un PSP réel.
