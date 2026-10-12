# Dépannage — jr-05-testing

Relever la première erreur, la commande, la version et le contexte. Une correction conserve le comportement demandé et ajoute ou rejoue une preuve ; retirer une assertion ne répare pas le problème.

| Symptôme | Cause plausible | Diagnostic discriminant | Résolution attendue |
|---|---|---|---|
| Les tests passent seuls mais pas ensemble | Mémoire, mocks ou timers partagés | Exécuter deux tests en ordre inverse et inspecter la fixture | Créer ou remettre à zéro les ressources dont chaque cas est propriétaire. |
| Un test vérifie le mock plutôt que le service | La frontière remplacée est trop large | Identifier le code réellement exécuté entre entrée et assertion | Conserver la vraie règle métier dans le test qui prétend la vérifier. |
| Playwright attend une page qui ne démarre pas | Le serveur n’a pas l’URL ou le port prévu | Lire les logs de démarrage et l’URL de disponibilité | Configurer webServer et vérifier les commandes avant le test. |
| E2E vert alors que Java est arrêté | Les routes sont toutes interceptées | Observer les requêtes et désactiver l’interception du scénario traversant | Utiliser le vrai backend pour le scénario qui vérifie l’intégration locale. |
| Le test de webhook expire selon l’heure | Le helper signe un timestamp ancien figé hors du test | Comparer Clock de validation et horodatage signé | Construire les valeurs à partir de l’horloge du cas. |
| La mutation ne fait échouer aucun test | L’invariant n’est pas asserté | Identifier l’observation qui devrait changer | Ajouter une assertion de comportement avant de conclure que le risque est couvert. |
| verify.sh finit avec succès après une erreur | Le script ignore le code de sortie ou poursuit malgré tout | Faire échouer volontairement une commande de test | Propager l’échec et conserver le rapport disponible. |

Si un port est occupé, identifier le processus avant de l’arrêter. Ne jamais arrêter globalement toutes les JVM ou tous les processus Node d’un poste partagé. Si une commande dépend d’un fichier demandé dans l’atelier, le construire d’abord : les dépôts élèves sont des supports, pas des solutions prêtes à lancer.
