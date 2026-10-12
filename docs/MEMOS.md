# Mémos 05 — Une preuve a un périmètre

## Choisir la bonne distance

Un test proche d’une règle explique vite son échec. Un test plus large observe davantage de composants mais demande plus de préparation. Le cours garde le calcul et les transitions en mémoire, le contrat au niveau MVC, l’interaction dans les tests React et une traversée complète dans Playwright. Cette répartition est une décision fondée sur les risques du fil rouge, pas une formule universelle.

## Un double remplace une frontière précise

Un mock qui renvoie toujours SUCCEEDED supprime le cas PENDING que l’écran doit pourtant gérer. Un test qui mocke tout le service paiement ne prouve pas son idempotence. Nommer la frontière remplacée rend les limites visibles : « composant React + API simulée » et « navigateur + API Java locale réelle » sont deux preuves différentes.

MockMvc traite les requêtes par l’infrastructure MVC et n’ouvre pas, à lui seul, un serveur réseau. Pour vérifier le port, le proxy et les échanges réels, conserver une traversée avec des processus démarrés. [Spring MockMvc](https://docs.spring.io/spring-framework/reference/testing/mockmvc.html).

## Isolation : pouvoir déplacer un test

Un test doit pouvoir s’exécuter seul et ne pas dépendre d’un précédent. Registre, horloge, timers et compteurs ont donc un propriétaire clair. Dans le navigateur, chaque scénario crée ses propres références. Si l’ordre des tests change le résultat, la preuve contient probablement une dépendance cachée qu’il faut identifier.

## Tester une interface par ses éléments utilisables

Un test utilisateur trouve un bouton par son rôle et son nom, l’active et observe un résultat. Il tolère ainsi une réorganisation du JSX qui préserve le comportement. Un sélecteur de classe CSS peut être nécessaire dans certains cas, mais ne devrait pas masquer un contrôle dépourvu de nom accessible. [Testing Library](https://testing-library.com/docs/guiding-principles/) · [Requêtes](https://testing-library.com/docs/queries/about/).

## Attendre une condition au lieu d’espérer

Playwright sait attendre certains états des éléments et ses assertions asynchrones peuvent retenter jusqu’à la condition ou au timeout. Les utiliser évite des pauses fixes dépendantes de la vitesse du poste. Le scénario doit aussi isoler ses données et annoncer ses attentes. [Playwright](https://playwright.dev/docs/best-practices).

## La mutation pose une question directe

Retirer une garde de montant puis voir le test échouer démontre que ce défaut précis est détecté. Cela ne démontre pas que tous les défauts similaires sont couverts. Une suite avec beaucoup d’assertions peut rester aveugle si elles ne touchent pas l’invariant. Le compte rendu relie donc mutation et test, sans annoncer une « couverture totale ».

## Rapport, build et preuve d’exécution

Un rapport montre ce qui a été exécuté et à quel résultat. Le build produit une sortie ; il ne résume pas tous les tests. Un screenshot montre un écran à un instant donné. Le journal relie ces éléments au même commit et dit explicitement lorsqu’un environnement a empêché une preuve. Cette traçabilité rend la revue possible.

## Questions de compréhension

1. Quel test faut-il lire en premier si le prix est faux alors que l’écran fonctionne ?
2. Que perd-on en interceptant toutes les requêtes dans le seul test E2E ?
3. Pourquoi isoler l’horloge plutôt que ralentir les tests ?
4. Quelle régression de code vos rapports montrent-ils réellement détectée ?
5. Quelle affirmation ne pouvez-vous toujours pas faire avec une suite verte ?
