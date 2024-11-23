# Réseaux Neuronaux
Un **réseau de neurones** est un modèle mathématique et informatique inspiré du fonctionnement des neurones biologiques dans le cerveau humain. 

## Le perceptron

Le perceptron est la base du réseau de neurone. C'est une fonction qui prendra plusieurs données en entrée et calculera une sortie. Pour créer un réseau de neurone on mettra bout à bout plusieurs perceptrons afin d'obtenir un réseau de neurone.

- voici une liste des éléments composants un perceptron :
  -  un perception peut avoir plusieurs entrées $X_1,X_2,...,X_n$
  -  chaque entrée aura un poids associé qui sera calculé durant la phase d’entraînement :  $W_1,W_2,...,W_n$
  - Une marge d'erreur  : $\epsilon$
  - la sortie : $y=\epsilon\sum_{i=0}^n X_iWi$

On peut alors vectoriser les entrées et les poids et faire un produit scalaire dans le but d'obtenir la sortie $y$

$\vec{X}=\begin{bmatrix}
X_1 \\ X_2 \\ ... \\X_n
\end{bmatrix}$

$\vec{W}=\begin{bmatrix}
W_1 \\ W_2 \\ ... \\W_n
\end{bmatrix}$

$y=\epsilon.\vec{W}.\vec{X}=\epsilon\sum_{i=0}^n X_iWi$

## La fonction d'activation

On dit qu'un perceptron est actif si la valeur de sortie est très élevée.
Etre élevée est relatif . On passe donc la valur de $y$ dans une fonction d'activation $f$  afin d'harmoniser le resultat. 

$f(y)=\frac{1}{1+e^{-y}}$

Cette fonction est définie entre $\forall x \in [-\infty;;+\infty;] , y \in [0;1]$ 