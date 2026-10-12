# Prérequis — Fullstack · Une chaîne de tests qui détecte les régressions

Importer dans le fork courant l’application, les tests et l’émetteur local des labs précédents avant la séance. Préinstaller Playwright et son navigateur compatible ; vérifier que le poste peut démarrer les deux services locaux. Une installation système impossible se documente avant le départ afin que l’encadrant fournisse un poste ou une exécution de secours identifiée.

## Reprise avant séance

Avant cette séance, importer votre réalisation validée de `java-react-04-webhooks-eleve` dans votre nouvelle copie `java-react-05-testing-eleve`. Reprendre backend/, frontend/ et scripts/ lorsqu’ils existent, ainsi que leurs configurations et lockfiles. Exclure .git/, target/, node_modules/, dist/, caches et sorties privées. Garder les documents de ce nouvel atelier. Noter le commit de provenance et exécuter les contrôles disponibles avant le début des 270 minutes.

## Poste prêt avant les 270 minutes

Utiliser un terminal Bash sous Linux, macOS ou WSL2. Installer JDK **21**, Maven **3.9.x**, Git, `curl`, Node **22.x au moins 22.12** et npm compatible avec ce Node. Vérifier la contrainte du Vite retenu dans sa documentation et dans `package.json`. Le cours choisit Java 21, Spring Boot 4.1.1, React et TypeScript pour apprendre ; l’annonce source ne prescrit pas ces versions ni ces bibliothèques annexes.

```bash
java -version
javac -version
mvn -version
node --version
npm --version
git --version
curl --version
```

Maven doit utiliser le même JDK 21 que `javac`. Figer les versions exactes dans le POM et conserver `package-lock.json` ; l’encadrant valide l’ensemble avant la séance et n’en change pas les versions majeures pendant celle-ci. Précharger les dépendances et, avant le lab 05, le navigateur Playwright. Les téléchargements et installations initiales sont hors du temps annoncé.

Prévoir deux terminaux, un éditeur, un navigateur récent, les ports locaux 8080 et 5173 disponibles, et un dossier `preuves/`. Aucun compte cloud, aucun compte PSP, aucune clé d’API et aucune carte ne sont requis. Les suites de test ne dépendent pas d’un service de paiement sur Internet.
