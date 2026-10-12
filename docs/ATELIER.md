# Lab 05 — Une chaîne de tests qui détecte les régressions

**4 h 30 · Java et React.** Une suite verte n’est utile que si elle échoue pour une erreur que l’équipe veut éviter. Partir de vos tests existants, choisir les risques encore mal protégés, puis montrer deux défauts détectés avant de rendre la version corrigée.

## 1. Relier un risque à un niveau de test — 25 min

Remplir la matrice du backlog. Choisir les preuves où le défaut se voit le plus directement ; réserver le navigateur aux interactions qu’il permet réellement de vérifier.

| Risque du fil rouge | Vérification minimale utile |
|---|---|
| Montant calculé depuis une entrée client | Test du service + test HTTP d’un champ ajouté |
| Création multiple lors d’un rejeu | Test Java de registre et concurrence contrôlée |
| Événement altéré accepté | Test HTTP avec corps brut modifié après signature |
| Terminal écrasé par un événement tardif | Test de transition avec deux eventIds |
| Succès affiché trop tôt | Test React sur réponse 202/PENDING |
| Nouvelle clé lors d’une reprise | Test React inspectant les deux requêtes |
| Timer encore actif après sortie | Test React avec timers contrôlés |
| Proxy, sérialisation et écran ne fonctionnent pas ensemble | Parcours Playwright sur les vrais services locaux |

Chaque ligne précise ce que le test prouve et ce qu’il laisse hors champ. Le nombre de tests n’est pas l’objectif ; la couverture de ces risques l’est.

## 2. Isoler les preuves Java — 65 min

Créer un service, un registre et une horloge propres à chaque cas qui en a besoin. Les identifiants peuvent être générés mais les assertions utilisent les références retournées, pas une valeur aléatoire prédéfinie. Le test de temps utilise `Clock.fixed`. Le mock PSP est local et son compteur est réinitialisé entre tests.

Renforcer les tests existants sans réécrire deux fois la même règle. Le service couvre calcul, intention et transitions ; MockMvc couvre routage, validation, statuts, headers et JSON. Ne mocker pas le service métier dans le seul test censé prouver qu’un champ montant falsifié est refusé de bout en bout dans l’API.

Pour la concurrence, utiliser une barrière de départ et attendre les résultats avec un timeout borné. Le résultat attendu est une seule création logique, pas simplement « aucune exception ». Pour les événements, isoler chaque paiement de manière à ce qu’un test terminal ne dépende pas d’un autre. Un reset de registre fourni uniquement aux tests ne devient pas un endpoint public effaçant les données.

Lancer la suite et conserver le rapport utile :

```bash
mvn -f backend/pom.xml -B --no-transfer-progress clean verify
```

En cas d’erreur, lire d’abord le premier échec causal. Une faute de compilation, un test non découvert et une assertion métier sont trois problèmes différents.

## 3. Tester React par ce que l’utilisateur peut faire — 55 min

Compléter les tests du formulaire et du paiement avec Testing Library. Préférer les rôles et noms accessibles : bouton, groupe, champ, statut et alerte. Employer un simulateur de réponses ou un double du module API qui respecte exactement les objets et statuts du contrat. Si tous les composants reçoivent une commande en props sans appeler le module API, vos tests ne prouvent pas que le JSON d’envoi est correct.

Couvrir au moins ces six comportements déjà introduits : catalogue indisponible puis repris, formulaire invalide, confirmation issue du serveur, 202/PENDING non terminal, reprise avec clé stable, polling arrêté à l’état terminal ou au démontage. Les réponses en attente sont des promesses contrôlées ; les timers du suivi sont pilotés par les outils de test. Restaurer doubles et timers après chaque cas.

Un test ne doit pas attendre arbitrairement cinq secondes pour « laisser React finir ». Attendre un élément ou une condition identifiée. Garder le texte attendu assez précis pour détecter un faux succès, sans figer une ponctuation sans importance dans tous les tests.

```bash
npm --prefix frontend run test -- --run
```

## 4. Prouver une traversée complète dans le navigateur — 55 min

Créer un scénario Playwright qui démarre ou attend le backend Java et Vite sur leurs URLs locales. La préparation du navigateur doit être faite avant le lab. Une configuration de serveur doit vérifier une URL prête à répondre et arrêter les processus qu’elle a créés.

Le scénario principal fait réellement : ouvrir la page, choisir quantité 2, créer la commande, choisir un moyen simulé, demander la tentative, lire sa référence affichée, envoyer un événement valide par un helper local du test, observer le résultat terminal dans l’écran. Dans ce scénario, ne remplacer ni `/api/orders` ni `/api/payments` par une interception qui retournerait tout le succès attendu : cela supprimerait la traversée Java que l’on veut vérifier.

Le helper HMAC appartient au processus de test ; il vise uniquement `127.0.0.1` et utilise la clé publique du cours. Il signe les octets envoyés. Le navigateur ne collecte aucune donnée bancaire. Chaque scénario crée ses propres commande, clé et eventId. Ne supposer pas que la base mémoire est vide, et ne supprimer pas les données d’autres scénarios.

Ajouter un second scénario ciblé si le temps le permet : double action puis même référence, ou erreur réseau contrôlée et reprise. Les scénarios supplémentaires sont utiles uniquement s’ils couvrent un risque manquant. Un happy path réellement traversant est requis ; un deuxième test fragile n’apporte pas de points automatiques.

Déclarer `test:e2e` dans le package frontend, puis exécuter :

```bash
npm --prefix frontend run test:e2e
```

Conserver rapport et trace en cas d’échec. Distinguer clairement les preuves composant avec API simulée de ce test navigateur avec API réelle locale.

## 5. Montrer que la suite sait refuser — 40 min

Introduire une régression Java ciblée : accepter un montant webhook incohérent, ou retourner une nouvelle tentative pour un rejeu. Lancer le test qui protège ce risque, conserver l’assertion rouge et expliquer pourquoi elle correspond au problème. Corriger et relancer la même preuve.

Introduire ensuite une régression React : afficher « Paiement simulé réussi » dès la réponse 202, ou régénérer la clé de reprise. Conserver l’échec du test de comportement, corriger puis rejouer. Ces mutations restent locales ou sur une branche de travail identifiée ; la version rendue contient les corrections. Ne remplacer pas ces expériences par une assertion artificielle `expect(false).toBe(true)`.

Pour chaque incident, écrire : modification fautive, risque utilisateur, test qui la détecte, diagnostic et correction. Si aucun test n’échoue, c’est un résultat utile : compléter la protection avant d’annoncer le risque couvert.

## 6. Automatiser une exécution lisible — 30 min

Créer `scripts/verify.sh` ou un équivalent simple qui exécute les contrôles du projet, propage un code de sortie non nul au premier échec et ne masque aucune erreur. Prévoir Java, tests React, build frontend et parcours navigateur selon un ordre documenté. Les dépendances sont restaurées avec `npm ci` à partir du lockfile ; aucune installation « latest » ne se glisse dans une vérification.

La commande doit marcher depuis un clone contenant uniquement les fichiers suivis et des outils préparés. Conserver les rapports même lorsqu’un test échoue. Un workflow CI est une extension possible si un dépôt réel et des droits adaptés existent ; aucune URL de run n’est exigée quand seule l’exécution locale a eu lieu.

Rendre matrice, script, rapports, deux défauts rouge puis vert, commit et limites. Arrêter les serveurs de test. Expliquer enfin une panne réelle que cette suite ne détecterait pas, par exemple une indisponibilité d’un prestataire qui n’existe pas dans notre simulation.

## Déroulé minuté — 270 minutes

| Plage | Activité | Durée |
|---|---|---:|
| 00:00–00:25 | Choisir les risques et construire une matrice de vérification | 25 min |
| 00:25–01:30 | Isoler mémoire, temps et mock puis compléter les tests Java | 65 min |
| 01:30–02:25 | Compléter les tests React par comportements et réponses contrôlées | 55 min |
| 02:25–03:20 | Écrire un parcours Playwright sur frontend et API réels locaux | 55 min |
| 03:20–04:00 | Déclencher deux régressions, diagnostiquer puis corriger | 40 min |
| 04:00–04:30 | Automatiser les commandes, conserver les rapports et faire relire | 30 min |

## Contrat commun du fil rouge

La boutique **PilotCrew Démo** ne vend rien. Elle présente le produit `demo-ticket`, libellé « Atelier fictif », à `unitAmountMinor: 2500`, devise `EUR`, `fictional: true`. Une quantité entière de **1 à 5** produit une commande dont le montant est calculé par le serveur. Dans cette simulation EUR seulement, 2500 unités mineures s’affichent 25,00 € fictifs. Aucun autre nombre de décimales ni aucune conversion de devise n’est traité.

| Objet | Champs publics attendus |
|---|---|
| Product | `id`, `label`, `unitAmountMinor`, `currency`, `fictional` |
| Order | `id`, `productId`, `quantity`, `amountMinor`, `currency`, `fictional` |
| Payment | `id`, `orderId`, `amountMinor`, `currency`, `method`, `status`, `fictional` |

Le corps de création de commande accepte uniquement `productId` et `quantity`. Le corps de création de paiement accepte uniquement `method`. Un montant, une devise ou un statut ajoutés par le client ne doivent jamais devenir une autorité : **ce contrat les refuse en 400**. Le code utilise les identifiants retournés, jamais un identifiant d’exemple comme s’il existait déjà.

| Route | Résultat contractuel |
|---|---|
| `GET /api/products` | 200, tableau de produits fictifs |
| `POST /api/orders` | 201, commande créée et en-tête Location |
| `GET /api/orders/{id}` | 200, commande ; 404 si référence absente |
| `POST /api/orders/{id}/payments` | À partir du lab 03 : 202, tentative PENDING ; Idempotency-Key obligatoire |
| `GET /api/payments/{id}` | À partir du lab 03 : 200, état serveur ; 404 si référence absente |
| `POST /api/webhooks/mock-psp` | À partir du lab 04 : réception d’un événement signé par le simulateur local |

Les erreurs structurées emploient `application/problem+json` et au moins `type`, `title`, `status`, `detail`, `code`. `code` est une extension de ce projet. Le texte est destiné à l’utilisateur ; aucune pile d’exception ne sort dans la réponse. Les codes HTTP du cours sont des choix de contrat précis ; ils ne décrivent pas toutes les API de paiement existantes.

Toutes les données métier sont en mémoire dans une JVM : un redémarrage les efface. Cette limite est visible dans la documentation et dans les scénarios. Le cours ne met en place ni identité client, ni base durable, ni gestion de stock, ni remboursement, ni autorisation financière réelle.
