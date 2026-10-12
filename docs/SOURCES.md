# Sources officielles et choix du laboratoire

Pages primaires consultées le **12 octobre 2026**. Les tutoriels, scénarios, contrats et barèmes sont des créations pédagogiques originales de PilotCrew. Les pages ci-dessous documentent les outils ou principes utilisés ; elles ne décrivent pas notre mock PSP.

| Source primaire | Usage dans le cours |
|---|---|
| [Spring Framework — MockMvc](https://docs.spring.io/spring-framework/reference/testing/mockmvc.html) | Périmètre des tests MVC sans serveur HTTP externe. |
| [Spring — Testing the Web Layer](https://spring.io/guides/gs/testing-web) | Tests HTTP de la couche MVC avec JUnit et MockMvc. |
| [Vitest — Getting Started](https://vitest.dev/guide/) | Exécution des tests frontend et intégration au projet Vite. |
| [Testing Library — Guiding Principles](https://testing-library.com/docs/guiding-principles/) | Tester le comportement observable depuis les éléments de l’interface. |
| [Testing Library — About Queries](https://testing-library.com/docs/queries/about/) | Rechercher par rôle et nom accessible ; choisir les assertions asynchrones. |
| [Playwright — Best Practices](https://playwright.dev/docs/best-practices) | Isolation des scénarios et assertions qui attendent l’état de la page. |
| [Playwright — Web server](https://playwright.dev/docs/test-webserver) | Démarrage de serveurs locaux et attente de disponibilité avant les scénarios. |
| [React — useEffect](https://react.dev/reference/react/useEffect) | Nettoyage des effets, dépendances et protection contre les réponses tardives. |
| [Spring Boot — tests d’application](https://docs.spring.io/spring-boot/reference/testing/spring-boot-applications.html) | Annotations et imports de tests compatibles avec Spring Boot 4.1.1 ; version de JUnit gérée par le parent. |
| [JUnit — guide utilisateur](https://docs.junit.org/current/user-guide/) | Tests Jupiter, assertions et outillage ; utiliser la version gérée par le projet. |

Les versions et noms d’API pouvant évoluer, l’encadrant vérifie les liens et la combinaison exacte avant une nouvelle promotion. Java 21 et Spring Boot 4.1.1 sont fixés pour ce cursus ; les versions JavaScript sont celles du package-lock.json validé. La version de JUnit est gérée par le parent Spring Boot ; le cours ne prescrit pas de mélanger une ancienne configuration de tests avec Boot 4.

La répartition Java / React, les interfaces de paiement, l’intégration et la rigueur sont des thèmes issus de [l’annonce de référence](https://consultant.jean-michel.io/annonces/31738). Le choix Spring Boot, TypeScript, Vite, tests et protocole HMAC local appartient à la conception pédagogique. Aucune source ne transforme les 27 heures de ce parcours en expérience senior.
