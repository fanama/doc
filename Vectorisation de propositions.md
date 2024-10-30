# Vectorisation de phrases

## Objectif
Analyser des phrases dans le but de détecter des intentions , de classifier des phrases et de générer une base de règles à partir d'un texte donné

## Etat des lieux : composition d'une proposition
Dans un  premier il est important  d'étudier la structure d'une proposition. Une proposition est formé d'un sujet, d'un verbe et d'un complément.

- exemple : "Le chat mange ses croquettes"
  - sujet : le chat
  - verbe : mange
  - complément : croquettes

## Idées

Dans le but d'analyse de propositions nous pouvons analyser chaque composants de la proposition. Nous pouvons vectoriser chaque composantes en différents paramètres.

### Sujet

Le sujet peut être vectorisé selon les paramètres suvants : 

- nature : c'est la nature du sujet. On prut le decliner en 3 éléments [objet, animal, human]

 Pour l'exemple vu plus haut on aurait chat=(2) 

 ### Verbe

Le verbe peut être vectorisé selon les paramètres suvants : 

- pronom : le pronom utilisé par le verbe. On peut le decliner en 6 éléments [je,tu,il,nous,vous,ils]
- temps : c'est le temps utilisé. Voici les éléments [passé,présent,future]
- nature : c'est le nature du verbe utilisé. Voici les éléments [action,etat]

 Pour l'exemple vu plus haut on aurait mange=(3,2,1) 

  ### Complément

Le Complément peut être vectorisé selon les paramètres suvants : 

- intensité : c'est l'intensité' Complément utilisé. Voici les éléments [-00;+00]
- nature : c'est le nature du Complément utilisé. Voici les éléments [objet,etat]

 Pour l'exemple vu plus haut on aurait les croquettes=(100,1) 