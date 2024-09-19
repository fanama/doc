## Objectif

Analyser les agents LLM dans le but de les utiliser dans nos futurs projets.

## Contexte

Ce document est une analyse des agents LLM. L'objectif est de voir comment nous pourrons les intégrer dans nos futurs projets.

## Analyse des composants Basiques

Les agents LLM sont composés de 4 éléments clés :

- **Cerveau**
  - Le cerveau sert d’orchestrateur. Il lie les différents composants entre eux.
- **LLM**
  - Le module LLM a pour objectif, à partir du prompt et d’une entrée utilisateur, de générer du texte ou des données.
- **Module de planification**
  - Le module de planification décompose la tâche en sous-tâches, générant ainsi un plan d’action.
- **Module de mémorisation**
  - Le module de mémorisation enregistre et récupère diverses informations et l’historique de l’agent.
    - **Mémoire court terme** : Historique des tâches en cours.
    - **Mémoire long terme** : Toutes les logs depuis la création (Base de données).
- **Module d’outillage**
  - Le module d’outillage exécute les tâches en utilisant des services externes.
    - **Tools request** : Liste des identifiants de tools et des paramètres associés.
    - **Tools services** : Liste des tools liés avec des fonctions d’exécution.

## Analyse du fonctionnement

- L’agent LLM prend en entrée un verbatim.
- Celui-ci doit ensuite être analysé et décomposé.
- Chaque sous-étape est ensuite appairée à une fonction tool après extraction des paramètres depuis l’historique de conversation.
- Les fonctions doivent être exécutées les unes après les autres.
- Le résultat final est enfin retourné à l’utilisateur.

### Résultats des premiers tests

Lors des premiers tests effectués, les résultats étaient concluants. Nous avons pu tester sur le use case de synthèse. Néanmoins, l’utilisation de l’agent reste assez compliquée. Il est nécessaire de passer en argument des tools en plus du prompt de base. Il serait plus intéressant si les tools étaient détectés automatiquement par le LLM lors de la génération du plan.

### Résultat du second test

Le second test était tout aussi concluant que le premier. Nous avons choisi de retirer les functions tools_request afin de laisser le LLM les sélectionner automatiquement à partir du prompt du client uniquement. Cela nécessite de préparer des functions-tools plus précises et des prompts tout aussi précis. L’agent LLM générera d’abord le plan, comme avant, mais sera capable de lier les différentes actions avec les fonctions associées puis exécutera le plan.

## Implementation dans nos projets

Pour utiliser les agents LLM dans nos projets, il sera nécessaire d’implémenter toute cette logique d’agent. Pour cela, je propose d’étendre l’entité projet en entité agent. Le projet ne se contentera alors plus d’appeler un unique LLM mais pourra appeler plusieurs LLM différents et même des outils externes.

- Le controller du chatbot ne se contentera plus d’appeler le LLM mais servira de cerveau. Il générera le plan d’action et exécutera chaque fonction tool.
- Il faudra ajouter un module d’outillage où les fonction tools seront créés pour les appels à divers services externes.
  - Il sera possible d’ajouter des tools.

Toutefois, il ne faut pas perdre le fonctionnement actuel. Il est donc nécessaire de laisser le choix à l’administrateur, lors de la création du projet, entre un simple proxy LLM et un agent complet.

## Analyse du fonctionnement dans nos projets

- Le projet prend en entrée un verbatim et un token.
- Le token redirige la requête vers le projet correspondant.
- La requête est ensuite envoyée dans un agent.
- Le projet a un prompt prédéfini afin que l’agent puisse faire un plan d’action lié aux outils.
- Chaque sous-étape est ensuite appairée à un tool par l’agent.
- Les fonctions doivent être exécutées les unes après les autres. Les paramètres d’une action correspondent aux résultats des actions précédentes, sauf pour la première action qui prendra directement l’input utilisateur.
- Le résultat final est enfin retourné à l’utilisateur.

## Liste des fonctions de l’agent

- **Planification** : L’agent doit pouvoir générer un plan d’action à partir d’un verbatim. Chaque action doit être liée à une fonction. Les éléments principaux sont :
  - Décomposition de l’objectif.
  - Génération de plan.
  - Sélection des actions.
  - Intégration de la mémoire et des outils.
- **Appels aux fonctions** : L’agent doit pouvoir appeler les fonctions associées aux actions de manière agnostique. Les éléments concepts clés sont :
  - Appel de fonction pure.
  - Définition de l’outil.
  - Sélection du sous-ensemble d’outils le plus pertinent pour l’étape de traitement en cours.
- **Parser de paramètres** : Lors de chaque appel aux fonctions, il est nécessaire de s’assurer que les paramètres d’entrée sont au bon format.
  - **Formatage des paramètres** : L’agent doit pouvoir formater les paramètres afin de les adapter aux function tools. Les paramètres peuvent être importés depuis la mémoire courte.
- **Mémoire courte** : L’agent doit pouvoir garder en mémoire les actions et résultats obtenus durant une session. C’est-à-dire depuis l’intervention de l’utilisateur jusqu’à la réponse finale. Les composants clés sont :
  - Mémoire de la conversation.
- **Mémoire longue** : L’agent doit pouvoir garder en mémoire (potentiellement en base de données) toutes les actions et résultats. C’est-à-dire depuis le lancement du projet.

## Liste des Outils pour nos projets

Ceci est une liste des outils qui peuvent être implémentés dans nos projets.

- **Outil par défaut** : C’est l’outil qui sera appelé si aucune fonction n’est associée à l’étape correspondante lors de l’étape de planification.
- **Parser de JSON** : C’est un outil dédié à répondre au format JSON.
- **API de récupération de conversation** : Cet outil appellera le chatbot dans le but de récupérer correctement les conversations à partir d’un ID (synthèse).
- **Synthétiseur** : Un outil permettant de synthétiser une conversation (synthèse).
- **Input waiter** : Cet outil permet d’attendre l’input de l’utilisateur (désambiguïsation).
- **Question generator** : Cet outil permet de générer des questions (désambiguïsation).
- **Question reformater** : Cet outil permet de reformuler les questions générées en prenant en compte l’historique de la conversation (désambiguïsation).
- **Synthétiseur V2** : Un outil permettant de synthétiser une conversation (désambiguïsation).

## Outils existants

- **Planification** :
  - ReAct
  - LangChain : LLMSingleActionAgent
  - LLamaIndex
- **Appel de fonction** :
  - MRKL
  - Toolformer
  - FunctionCalling
- **Module de mémoire courte** :
  - Langchain
  - LlamaIndex
  - HayStack
- **Module de mémoire longue** :
  - LangChain
  - LlamaIndex
  - MRKL
