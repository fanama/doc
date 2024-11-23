# Réseaux Neuronaux
Un **réseau de neurones** est un modèle mathématique et informatique inspiré du fonctionnement des neurones biologiques dans le cerveau humain. 

## Le perceptron

Le perceptron est la base du réseau de neurone. C'est une fonction qui prendra plusieurs données en entrée et calculera une sortie. Pour créer un réseau de neurone on mettra bout à bout plusieurs perceptrons afin d'obtenir un réseau de neurone.

- voici une liste des éléments composants un perceptron :
  -  un perception peut avoir plusieurs entrées $X_1,X_2,...,X_n$
  -  chaque perceprion aura un poids associé qui sera calculé durand la phase d'entrainnement :  $W_p$
  - Une marge d'erreur  : $\epsilon$
  - la sortie : $y=W\sum_{i=0}^n X_i$