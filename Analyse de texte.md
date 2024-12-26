# Analyse de texte

## Objectif

L'objectif est la simulation de la compréhension humaine par un système non humain.

## Definition

**Comprendre** : saisir ou se rendre compte de quelquechose. C'est être capable de l'expliquer ou de le répliquer

**Expliquer** : pour expliquer un concept on peut passer par la définition du dictionnaire et un exemple

## L'approche graphique

Nous pouvons grouper les mots entre eux dans le but d'obtenir des liens.
Ces mots et ces liens créent alors des graphes.

### Le graph de definition

Si on prend un mot on peut récupérer sa définition depuis le dictionaire.
On peut ensuite assoscier au mot d'origine chaque terme utilisé dans la définition.
À la fin on obtient une toile de définition du mot.

### Le graph de generation

Si on prend un corpus de texte on aura accès à tous les mots utilisés et ordonnés
dans un sens logique.
On peut alors assoscier chaque mot avec le mot qui le suit.
Dans un objectif de lisibilité, on n'affichera qu'une seule fois chaque terme. 
On mettra en evidence chaque association avec le mot suivant via une liaison.
À la fin on obtiendra une toile d'assosciation de mots.

## Algorithme

### Le graph de definition

Le graph de definition commence par un seul mot. 
On garde en mémoire les mots déjà défini. 
On reçois un premier mot à définir et on l'enregisre en mémoire.
Il faut ensuite récupérer sa définition depuis le dictionaire.
Chaque mot qui a servi à le définir doit être ajouté en mémoire. 
Si le mot est déjà en mémoire on passe au mot suivant. Sinon il faut
l'associer au mot actuel ,rechercher sa définition et refaire l'opération jusqu'à ce que tous
les mots soient définis.