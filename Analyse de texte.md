# Analyse de Texte

## Objectif

L'objectif est de simuler la compréhension humaine par un système non humain.

## Définition

**Comprendre** : Saisir ou se rendre compte de quelque chose. C'est être capable de l'expliquer ou de le répliquer.

**Expliquer** : Pour expliquer un concept, on peut passer par la définition du dictionnaire et un exemple.

## L'approche graphique

Nous pouvons regrouper les mots entre eux dans le but d'obtenir des liens. Ces mots et ces liens créent alors des graphes.

### Le graphe de définition

Si on prend un mot, on peut récupérer sa définition depuis le dictionnaire. On peut ensuite associer au mot d'origine chaque terme utilisé dans la définition. À la fin, on obtient une toile de définition du mot.

### Le graphe de génération

Si on prend un corpus de texte, on aura accès à tous les mots utilisés et ordonnés dans un sens logique. On peut alors associer chaque mot avec le mot qui le suit. Dans un objectif de lisibilité, on n'affichera qu'une seule fois chaque terme. On mettra en évidence chaque association avec le mot suivant via une liaison. À la fin, on obtiendra une toile d'association de mots.

## Algorithme

### Représentation du graphe

Un graphe de mots peut être représenté sous forme de dictionnaire de listes. Pour un corpus donné, on commence par créer un dictionnaire. À chaque nouveau mot rencontré, on crée une clé dans le dictionnaire portant le nom de ce mot. Le mot suivant sera ensuite ajouté comme valeur à la liste associée à cette clé.

### Le graphe de définition

Le graphe de définition commence par un seul mot. Dès qu'un mot est défini, on crée la clé associée dans le dictionnaire. Pour chaque mot utilisé dans la définition du mot d'origine, on réitère le processus de définition jusqu'à ce que tous les mots soient définis.

### Le graphe de génération

Le graphe de génération est plus simple à mettre en place. Il s'agira de récupérer un corpus et de regarder la succession de mots. Pour chaque nouveau mot rencontré, on ajoute à la clé associée le mot qui suit.

## liens

**réseaux**  https://realpython.com/python-ai-neural-network/