Api la Data Science pour tous -- Jour 1
===


# Python pour la Data Science

[TOC]

## Présentation

* Jean-Benoist Léger
    * Enseignant chercheur au GI - FDD

## Introduction

Langage objet, interprété, typé (typage fort mais dynamique)

IPython (`ipython3`) -> mieux que l'interface Python de base
- Sorties plus lisibles (erreurs)

(Jupyter) Notebook -> basé sur le même principe, pratique

Pas d'ordre dans l'exécution des cellules -> seul leur numéro est important

`_3` = variable stockant le résultat de la cellule 3 (également accessible avec `Out[3]`)

Créer son module : 
- Créer un fichier (`monmodule.py`) dans le même répertoire que le notebook
- Y écrire (`a = 1`) par exemple
- `import monmodule` dans le notebook (probablement faire attention à l'emplacement du fichier)
  
<!-- Il suffit de créer un fichier monmodule.py dans le bloc note avec a=1 dedans pour reproduire l'exemple -->
- `monmodule.a` : accéder à la variable `a` du module `monmodule` -> va afficher `1` à l'exécution

On peut vérifier le type de ses variables : 
```python
a = 12
print(type(a))  # <class 'int'>
a = "bob"
print(type(a))  # <class 'str'>
```

On peut à peu près tout mettre dans un nom de variable. En particulier :
`α = 12` OK
`☭ = 12` NOT OK

## Types numériques
On peut vérifier l'égalité de valeurs ou d'objet
```python
a = 500
b = 500
a == b  # True
```
MAIS `b is a` est `False` car `a` et `b` ne sont pas les mêmes objets, ils ont cependant la même valeur
```python
a = 1
b = a
b is a  # True
```
Attention aux nombres à virgule, le type flottant ne représente pas bien les réels

```python
a = 0.3 - 0.2 - 0.1
type(a)  # float
a == 0  # False
a  # -2.7755575615628914e-17
```

Des modules permettent de représenter des fractions

```python
from fractions import Fraction
Fraction(1, 10)  # 1/10
Fraction(3, 10) - Fraction(2, 10) - Fraction(1, 10)  # Fraction(0, 1)
```

## Séquences

Types : 
- `string` : chaîne de caractères

```python
"bonjour"
'bonjour'

'''
bonjour
salut
'''

machaine = "abc"
len(machaine) # 3
machaine[0] # a
machaine[1] = z  # ERREUR, objet immutable (on ne peut pas changer un caractère d'une string)

for i in machaine:
    print(i)
# Ces deux lignes de codes vont afficher :
'''
a
b
c
'''

c = "voila"
d = c
c += "truc" 
print(c)  # voilatruc
print(d)  # voila
```
- `tuple` : séquence **immutable** de références
```python
t = (1, 2, "bidule")
len(c)  # 3
(c, 1)  # ((1, 2, 'bidule'), 1)
c[0]  # 1
c[0] = 'ce tuto est vraiment super bien fait'  # Marche pas
(a, b, c, d) = (1, 2, 3, 4)
a  # 1
b  # 2
```

- `list` : séquence **mutable** de références
```python
c = [1, 2, "machin"]
c[0] = "vive la datascience" # OK
```

Cas particulier : 
```python
Hell = (1, 3, [1, 2, (10, 12), 1], 8)
Satan = Hell[2]  # [1, 2, (10, 12), 1]
Belzebuth = Satan[2]  # (10, 12)
```

- `set`

## Indexation des séquences

Borne supérieure **exclue**

```python
a = list(range(32,51)) # 32, 33, ..., 50
a[-1] # 50, dernier élément de la séquence
a[-2] # 49, avant-dernier élément ...
a[0:5] # 32, 33, 34, 35, 36 | 5 premiers éléments
a[5:] # 37, 38, ..., 50 | tous les éléments à partir du 6è
a[0:6:2] # 32, 34, 36 | les éléments entre les bornes 0 et 6 exclu avec un pas de 2
a[::-1] # 50, 49, ..., 32 | parcours de la liste en sens inverse
```

## Dictionnaires

```python
d = {"machin": 1, "a": 4, "s": 5}
d["machin"] # 1 
```

## Structures conditionnelles

- `if`

```python
a = 0
if a == 1 :
    print("je suis 1")
else :
    print("je ne suis pas 1")
```

- `while`

```python
a = 1
while a < 10 :
    print(a)
    a += 1

# Affichage :
'''
1
2
3
4
5
6
7
8
9
'''
```

- `for`
```python
for w in Hell :
    print(w)
    
# Affichage
'''
1
3
[1, 2, (10, 12), 1]
8
'''

for i, o in enumerate(Hell) :
    print(i, o)
# Affichage
'''
0 1
1 3
2 [1, 2, (10, 12), 1]
3 8
'''
``` 

## Fonctions

```python
def f(x, p=5):
    print(x)
    print(x + p)
    return x**p
    
f(2)
# Affichage
'''
2
7
32
'''
    
f(2, 2)
# Affichage
'''
2
4
4
'''

l = [0, 1, 2, 3, 4, 5]
[f(x) for x in l]
# Return : [0, 1, 32, 243, 1024, 3125]

# Lambda fonctions
f2 = lambda x: x+x
f2(6) # 12
```

## Python scientifique

Pleins de bibliothèques : `numpy scipy matplotlib`

### Numpy
```python
import numpy as np # on raccourcit numpy par np pour clarifier le code

v = np.array([1,2,3]) # array([1,2,3])
v.shape # (3,)

v = np.array([[1,2,3]]) # array([[1,2,3]])
v.shape # (1,3) # vecteur ligne

v = np.array([[1],[2],[3]])
v.shape # (3,1) # vecteur colonne

M = np.random.uniform(size=(3,3)) # Liste 3 par 3 de nombre aléatoires entre 0 et 1
'''
array([[0.95847666, 0.37154452, 0.1448102 ],
       [0.3609038 , 0.90655956, 0.0487145 ],
       [0.31224159, 0.62867489, 0.6089461 ]])
'''
K = M [::-1,1:]
K[0,0] = 1
print(K)
print(M)
# Affichage
'''
[[1.         0.7315426 ]
 [0.14062484 0.94975081]
 [0.93444254 0.87258297]]
[[0.86372201 0.93444254 0.87258297]
 [0.41473947 0.14062484 0.94975081]
 [0.1748971  1.         0.7315426 ]]
'''
```

### Matplotlib

```python
import matplotlib.pyplot as plt
xs = np.linspace(0,5,10000)
plt.plot(xs)
```
