Api la Data Science pour tous -- Jour 2
===



# Introduction aux principaux modèles
Apprentissage Automatique


[TOC]

## Présentation

* Sylvain Rousseau
    * Enseignant chercheur au GI - FDD

## Introduction

Présentation en deux parties

### Apprentissage non-supervisé

Exemples sans étiquettes (animaux, emails...)
Est-ce qu'il existe des groupes naturels ? Représentation plus simple ?

### Apprentissage supervisé

Exemples étiquettés, nature de l'information renseignée 
Cela englobe la **discrimination** (on a une information de classe en étiquette) et la **régression**  (on a un nombre en étiquette).
- photos de chien et chats avec caractère discriminatoire (oreilles pointues)
- maisons avec prix et description précise


## Apprentissage non-supervisé

Notation : **n** exemples avec chacun **p** valeurs (vecteurs **x~(i)~** de dimension p)



### K-means

But : Former des clusters, groupements naturels (par rapport à ce que l'humain trouve cohérent).

Pour ce faire, on fixe arbitrairement le nombre **k** de groupes a former puis on choisit **k** points de départ positionnés aléatoirement. On va former ainsi **k** clusters.

Exemple (dataset iris) :
![](https://i.imgur.com/dyvqrNn.png)

Construction itérative : on va associer chaque point avec un centre, on prend le centre de gravité du groupement de points et on déplace le centre à ce niveau (on colorie les points appartenant au groupement d'un centre par la couleur de ce centre)

On s'arrête quand le groupement est fixé (les centres ne bougent plus). On dit alors que l'algorithme a convergé. En pratique, il converge assez rapidement (dizaine d'itérations)

Intuitivement, on visualise bien l'algorithme k-means en 2 dimensions (un plan) mais on peut l'appliquer dans un espace à n dimensions (R<sup>n</sup>).

#### Remarques

Importance du choix du nombre de clusters

Dépendance par rapport aux points de départs choisis. Il existe des heuristiques pour la sélection des centres au début qui peuvent permettre une convergence plus rapide par exemple.

En pratique, on lance l'algorithme plusieurs fois et on garde les groupements minimisant l'inertie intra-classe (l'inertie dans chaque classe/groupe). En fait l'algorithme converge vers un minimum **local** de l'inertie intra-classe.

Inertie : mesure de la qualité du groupement en utilisant la distance centre/points (k-medoids)

### L'ACP (Analyse en composantes principales)

Jeu de données approchable en 1D/2D ? (points distribués "linéairement" (1D) dans un espace 2D)

But : représenter des individus/un jeu de données dans un espace de plus faible dimension.

* Avantages
     * Les nouvelles features sont décorrélées
     * Réduction du nombre de features sans perdre trop d'informations (ex compression images)
     * Intéressant pour la visualisation
     * Calcul des nouvelles features = diagonalisation de matrices en général (Diagonalisation de la matrice de variance)
* Limites
    * Projection linéaire (kernel PCA, *manifold learning* : apprentissage d'une droite au lieu d'une courbe)
    * Problème numérique lorsque n ou p est grand (*online* PCA)
    * Diagonaliser une matrice de très grosse dimension peut être couteux

## Apprentissage supervisé

Même contexte que tout à l'heure sauf qu'on ajoute une information de classe (label).

### K plus proches voisins (Knn)

Sert à la prédiction :

On connait le groupe de chaque exemple (donnée), et on cherche à prédire le groupe d'appertenance d'un nouveau point, les **k** voisins les + proches nous donnent le groupe du nouveau point.

### Algorithme bayésien naïf

* Algo de discrimination
* Basé modèle (on connait la forme du jeu de données)
* De type génératif (capacité de générer de nouveaux éléments de la classe)
* Repose sur la règle de Bayes
* Marche bien en pratique malgré hypothèse forte

Basé sur des probas conditionnelles

Règle de Bayes : $\mathbb{P}(C_k | features)=\frac{\mathbb{P}(features | C ~k~).\mathbb{P}(C_k)}{\mathbb{P}(features)}$

Ce qui nous permets de comparer chaque proba d'une classe par rapport à une autre (la + grande -> classe la + probable) :

$\frac{\mathbb{P}(C_i | features)}{\mathbb{P}(C_j | features)}$ = $\frac{\mathbb{P}(features | C_i).\mathbb{P}(C_i)}{\mathbb{P}(features | C_j).\mathbb{P}(C_j)}$

### Régression logistique

On revient à $Pr(class | features)$
Limité à deux classes
On cherche le meilleur **w** ($\frac{1}{e^{w.features}}$)

### SVM - Support Vector Machine

Chercher un séparateur linéaire dans un espace de dimension 2 (axe du tube)

Les vecteurs de support sont les exemples (points) qui supportent la marge (parois du tube)

Séparateur optimal ->  celui qui maximise la marge (distance entre centre du tube et ses parois)

Si jeu de données non-linéairement séparable ?
* Paramètre de tolérance $C$ : on autorise les exemples mal classés
* Vecteurs de support contenus dans la marge
    - Plus $C$ est grand, plus la marge est petite

Si jeu de données séparable, mais pas linéairement ?

On utilise un noyau (kernel) non linéaire

Si on donne un $C$ trop élevé, on peut se retrouver avec un modèle qui va vouloir être trop précis (va bien performer sur l'exemple d'entraînement, mais pas sur de nouvelles données -> surapprentissage)

Algorithme de discrimination ; basé sur des instances

* Avantages
    * Nécessite la résolution d'une minimisation quadratique linéairement contrainte
    * Flexible grâce aux noyaux
    * Extension à la régression
* Limites
    * Il faut choisir le noyau et ses éventuels paramètres
    * Le nb de vecteur de support n'est pas contrôlé : lenteur de prédiction

### Arbres de décision

Séparer les exemples avec un arbre jusqu'à obtenir une classification uniforme 

Critère de gini : mesure de la pureté des séparations (que des bleus dans la région bleue)

On s'arrête lorsque le critère de gini de toutes les feuilles de l'arbre est nul

Exemple de séparation d'un espace 2 dim : cumul de droites horizontales/verticales qui permettent de séparer les 2 classes

![](https://i.imgur.com/Uc9Bela.png)

Victime du problème du surapprentissage aussi, l'exemple ci-dessus montre qu'on a trop pris en compte des données aberrantes

On va élaguer l'arbre dans ce cas

### Forêt aléatoire (random forest)

Les arbres de décision font du surapprentissage, comment y remédier :
* On génère d'autres ensembles d'apprentissage par *bootstrap*
* On apprend des arbres de décision sur chaque ensemble d'apprentissage
* On décide par vote par majorité (aggregating):
    * Sur chaque zones de l'espace, on regarde les différents ensembles d'apprentissage
    * On regarde combien d'ensembles on choisit quelle couleur pour la région sélectionnée -> vote à la majorité
* On combine les arbres décisions et il en résulte la solution qui n'est pas victime de surapprentissage

### Réseau de neurones

Entrainé par rétro-propagation du gradient (*backpropagation*)
Difficile à entrainer : 
* Espace de paramètres grand (autant de paramètres que de données)
* Fonction de perte non convexe (on peut être coincé dans un minimum local ou dans une plaine)

Il faut décider du nombre de couches/neurones par couches
Paradoxalement, un réseau profond permet de mieux apprendre le modèle

## Apprentissage supervisé : régression

### Régression linéaire

Implique que l'on connait le modèle (simple) et les intervalles de confiance
Un seul prédicteur (1 caractéristique), valeur en ordonnée

### Arbre de régression

On crée une fonction en cherchant des séparateurs type **feature<C**

![](https://i.imgur.com/mlc9iV5.png)

-> Marche aussi avec la foret aléatoire

### Visualisation

Jeu de données MNIST : classique 
70.000 chiffres manuscrits, 28*28 = 784 descripteurs, 10 classes
Le but : construire un algorithme qui reconnaît les 10 classes "chiffre"

En périphérique les descripteurs ne sont pas discrimants => on peut limiter à une centaine des descripteurs qui nous intéresseront pour discriminer les différentes classes

La PCA (projection dans un espace de dimension 2, sans données de classe) peut servir pour visualiser les données.
Elle prend en compte les distances locales et les distances globales pour chaque exemple.


<!-- ceux qui ont le diapo, vous l’avez trouve ou svp ?  - disponible sur moodle a partir de 12h) -->