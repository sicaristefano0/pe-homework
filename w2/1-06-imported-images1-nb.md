---
jupytext:
  cell_metadata_json: true
  encoding: '# -*- coding: utf-8 -*-'
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.19.5
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# TP images (1/2)

merci à Wikipedia et à stackoverflow

```{admonition} disclaimer
:class: danger

**vous n'allez pas faire ici de traitement d'image - on se sert d'images pour égayer des exercices avec `numpy`  
(et parce que quand on se trompe: on le voit)**
```

+++

```{admonition} **Notions intervenant dans ce TP**
:class: tip

* création, indexation, slicing, modification  de `numpy.ndarray`
* affichage d'image (RBG, RGB-A, niveaux de gris)
* lecture de fichier `jpg`
* les autres notions utilisées sont rappelées (très succinctement)

**N'oubliez pas d'utiliser le help en cas de problème.**
```

+++

## import des librairies

+++

1. Importez la librairie `numpy`

1. Importez la librairie `matplotlib.pyplot`  
ou toute autre librairie d'affichage que vous aimez et/ou savez utiliser: `seaborn` ...

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt

plt.rcParams['figure.figsize'] = [4, 4]
```

2. optionnel - changez la taille par défaut des figures matplotlib
   par exemple choisissez d'afficher les figures dans un carré de 4x4 (en théorie ce sont des inches)

   ````{tip}
   il y a plein de façons de le faire, google et/ou stackoverflow sont vos amis...
   ````

+++

## création d'une image de couleur

````{admonition} Rappels (rapides)
:class: tip


* dans une image en couleur, les pixels sont représentés par leurs *dosages* dans les 3 couleurs primaires: `red`, `green`, `blue` (RGB)  
  ```{image} media/synthese-additive.png
  :width: 100px
  :align: right
  ```

* l'affichage à l'écran, d'une image couleur `rgb`, utilise les règles de la synthèse additive  
`(r, g, b) = (255, 255, 255)` donne la couleur blanche  
`(r, g, b) = (0, 0, 0)` donne du noir  
`(r, g, b) = (255, 0, 0)` donne du rouge
`(r, g, b) = (255, 255, 0)` donne du jaune ...

* pour afficher le tableau `im` comme une image, utilisez: `plt.imshow(im)`
* pour afficher plusieurs images dans une même cellule de notebook faire `plt.show()` après chaque `plt.imshow(...)`
````

+++

**Exercice**

1. Créez un tableau **non initialisé**, pour représenter une image carrée **de 91 pixels de côté**, d'entiers 8 bits non-signés, et affichez-le  
   ```{admonition} indices
   :class: tip

   * il vous faut pouvoir stocker 3 `uint8` par pixel pour ranger les 3 couleurs
   * on s'intéresse uniquement à la taille, et pas au contenu puisqu'on a dit "non initialisé"; que vous ayez du blanc, du noir ou du bruit, c'est OK
   ```

```{code-cell} ipython3
# votre code
import numpy as np
import matplotlib.pyplot as plt
image=np.empty(shape=(91,91,3), dtype = np.uint8)
```

2. Transformez le en tableau blanc (en un seul slicing) et affichez-le

```{code-cell} ipython3
# votre code
image [::,::]=[255,255,255]
plt.imshow(image)
```

3. Transformez le en tableau vert (en un seul slicing) et affichez-le

```{code-cell} ipython3
# votre code
image [::,::]=[0,255,0]
plt.imshow(image)
```

4. Affichez les valeurs RGB du premier pixel de l'image, et du dernier

```{code-cell} ipython3
print(f"premier pixel : rouge : {image[0,0,0]}, vert : {image[0,0,1]}, bleu : {image[0,0,2]} ")
print(f"dernier pixel : rouge : {image[-1, -1, 0]}, vert : {image[-1, -1, 1]}, bleu : {image[-1, -1, 2]}")
```

5. Faites un quadrillage d'une ligne bleue, toutes les 10 lignes et colonnes et affichez-le

```{code-cell} ipython3
# votre code
image[::10, :] = [0, 0, 255]   # les lignes horizontales
image[:, ::10] = [0, 0, 255]   # les colonnes verticales
plt.imshow(image)
```

## lecture d'une image en couleur

+++

1. Avec la fonction `plt.imread` lisez le fichier `data/les-mines.jpg`

```{code-cell} ipython3
# votre code
plt.imread("../data/les-mines.jpg")
```

2. Vérifiez si l'objet est modifiable avec `im.flags.writeable`; si il ne l'est pas, copiez l'image

```{code-cell} ipython3
image1 = plt.imread("../data/les-mines.jpg")
print(image1.flags.writeable)

if not image1.flags.writeable:
    image1 = image1.copy()
```

3. Affichez l'image

```{code-cell} ipython3
plt.imshow(image1)
```

4. Quel est le type de l'objet créé ?

```{code-cell} ipython3
# votre code
print(f"l'objet créé est de type, {type(image1)}, c'est un tableau numpy")
```

5. Quelle est la dimension de l'image ?

```{code-cell} ipython3
print(f"l'objet créé contient {image1.size} valeurs "
      f"({image1.shape[0]} x {image1.shape[1]} x {image1.shape[2]})")
```

6. Quelle est la taille de l'image en hauteur et largeur ?

```{code-cell} ipython3
print(f"l'image fait : {image1.shape[0]} pixels d'hauteur, {image1.shape[1]} pixels de largeur")
```

8. Quel est le type des pixels ?  
(deux types pour les pixels: entiers non-signés 8 bits ou flottants sur 64 bits)

+++

7. Quel est le nombre d'octets utilisé par pixel ?

```{code-cell} ipython3
# il y a trois octets par pixel
print(f"le type des pixels est {image1.dtype}, il y a trois octets par pixel")


```

9. Quelles sont ses valeurs maximale et minimale des pixels ?

```{code-cell} ipython3
# votre code
print(f"minimum : {image1.min()}, maximum : {image1.max()}")
```

10. Affichez le rectangle de 10 x 10 pixels en haut de l'image

```{code-cell} ipython3
# votre code
plt.imshow(image1[0:10,0:10])
```

## accès à des parties d'image

+++

1. Relire l'image

```{code-cell} ipython3
image2 = plt.imread("../data/les-mines.jpg")
```

2. Slicer et afficher l'image en ne gardant qu'une ligne et qu'une colonne sur 2, 5, 10 et 20  
(ne dupliquez pas le code)

```{admonition} indices
:class: tip

* vous pouvez créer plusieurs figures depuis une seule cellule  
  pour cela, faites plusieurs fois la séquence `plt.imshow(...); plt.show()`

* vous pouvez ensuite choisir de 'replier' ou non la zone *output* en hauteur;  
  c'est-à-dire d'afficher soit toute la hauteur, soit une zone de taille fixe avec une scrollbar pour naviguer  
  pour cela cliquez dans la marge gauche de la zone *output*
```

```{code-cell} ipython3
# votre code

for saut in [2, 5, 10, 20]:
    print(f"image sous-échantillonnée d'un facteur {saut}")
    plt.imshow(image2[::saut, ::saut])
    plt.show()
```

3. Isoler le rectangle de `l` lignes et `c` colonnes en milieu d'image  
affichez-le pour `(l, c) = (10, 20)`) puis `(l, c) = (100, 200)`

```{code-cell} ipython3
# votre code
sh=image2.shape
centreligne = sh[0] // 2
centrecolonne = sh[1] // 2

print("(l, c) = (10, 20)")
plt.imshow(image2[centreligne - 10 // 2: centreligne + 10 // 2,
                   centrecolonne - 20 // 2: centrecolonne + 20 // 2])
plt.show()

print("(l, c) = (100, 200)")
plt.imshow(image2[centreligne - 100 // 2: centreligne + 100 // 2,
                   centrecolonne - 200 // 2: centrecolonne + 200 // 2])
plt.show()
```

## canaux RGB de l'image

+++

1. Relire l'image

```{code-cell} ipython3
image3=plt.imread("../data/les-mines.jpg")
```

2. Découpez l'image en ses trois canaux Red, Green et Blue
   (Il s'agit donc de construire trois tableaux de dimension 2)

```{code-cell} ipython3
# votre code
rouge=image3[::,::,0]
green=image3[::,::,1]
blue=image3[::,::,2]
```

3. Afficher chaque canal avec `plt.imshow`; la couleur est-elle la couleur attendue ?  
    Si oui très bien, si non que se passe-t-il ?

    ```{admonition} **rappel** table des couleurs
    :class: tip

    * `RGB` représente directement l'encodage de la couleur du pixel, et non un indice dans une table
    * donc pour afficher des pixel avec les 3 valeurs RGB pas besoin de tables de couleurs, on a la couleur
    * mais pour afficher une image unidimensionnelle contenant des nombres de `0` à `255`, 
      il faut bien lui dire à quoi correspondent les valeurs  
      (lors de l'affichage, le `255` des rouges n'est pas le même `255` des verts)

    * du coup, voyez le paramètre `cmap=` de `plt.imshow`; et notamment avec `'Reds'`,  `'Greens'` ou  `'Blues'`
    ```

```{code-cell} ipython3
# votre code
print("rouge")
plt.imshow(rouge, cmap="Reds")
plt.show()
print("vert")
plt.imshow(green, cmap="Greens")
plt.show()
print("bleu")
plt.imshow(blue, cmap="Blues")
plt.show()
```

4. Corrigez vos affichages si besoin

```{code-cell} ipython3
# votre code
```

5. Copiez l'image, et dans la copie, remplacer le carré de taille `(200, 200)` en bas à droite:

   * d'abord par un carré de couleur RGB `(219, 112, 147)` (vous obtenez quelle couleur)  
   * puis par un carré blanc avec des rayures horizontales rouges de 1 pixel d'épaisseur

```{code-cell} ipython3
# votre code
cimage3=image3.copy()
cimage3[-200:,-200:]=[219,112,147]
plt.imshow(cimage3)
plt.show()

cimage3[-200:,-200:]=[255,255,255]
cimage3[-200::2,-200:]=[255,0,0]
carre = cimage3[-200:,-200:]
plt.imshow(cimage3)
plt.show()
```

6. enfin pour vérifier, affichez les 20 dernières lignes et colonnes du carré à rayures

```{code-cell} ipython3
# votre code
plt.imshow(carre[-20:,-20:])
```

## transparence des images

+++

````{admonition} rappel: la transparence
**rappel** RGB-A

* on peut indiquer, dans une quatrième valeur des pixels, leur transparence
* ce 4-ème canal s'appelle le canal alpha
* les valeurs vont de `0` pour transparent à `255` pour opaque
````

+++

1. Relire l'image initiale (sans la copier)

```{code-cell} ipython3
# votre code
image4=plt.imread("../data/les-mines.jpg")
```

2. Créez un tableau vide de la même hauteur et largeur que l'image, du type de l'image initiale, mais avec un quatrième canal

```{code-cell} ipython3
# votre code
dim = image4.shape
cimage4 = np.empty(shape=(dim[0], dim[1], 4), dtype=image4.dtype)
```

3. Copiez-y l'image initiale, mettez le quatrième canal à `128` et affichez l'image

```{code-cell} ipython3
# votre code
cimage4[:, :, 0] = image4[:, :, 0]
cimage4[:, :, 1] = image4[:, :, 1]
cimage4[:, :, 2] = image4[:, :, 2]
cimage4[:, :, 3] = 128

plt.imshow(cimage4.astype(np.uint8))
```

## image en niveaux de gris en `float`

+++

1. Relire l'image `data/les-mines.jpg`

```{code-cell} ipython3
# votre code
image5 = plt.imread("../data/les-mines.jpg")
```

2. Passez ses valeurs en flottants entre 0 et 1 et affichez-la

```{code-cell} ipython3
# votre code
image5_float = image5 / 255
plt.imshow(image5_float)
```

3. Transformer l'image en deux images en niveaux de gris :  
a. en mettant pour chaque pixel la moyenne de ses valeurs R, G, B  
b. en utilisant la correction `Y` (qui corrige le constrate) basée sur la formule  
   `Y = 0.299 * R + 0.587 * G + 0.114 * B`  
c. optionnel: si vous pensez à plusieurs façons de faire la question a., utilisez `%%timeit` pour les benchmarker et choisir la plus rapide

```{code-cell} ipython3
# votre code
#3a
gris_moyenne = image5_float.mean(axis=2)
plt.imshow(gris_moyenne, cmap="gray")
plt.show()
#3b
gris_Y = 0.299*image5_float[:,:,0] + 0.587*image5_float[:,:,1] + 0.114*image5_float[:,:,2]
plt.imshow(gris_Y, cmap="gray")
plt.show()
```

```{code-cell} ipython3
#3c
%%timeit
gris_moyenne = image5_float.mean(axis=2)
```

4. Prenez l'image de 3.a (moyenne des 3 canaux), passez les pixels au carré, et affichez le résultat
   Quel est l'effet sur l'image ?

```{code-cell} ipython3
# votre code
gris_carre = gris_moyenne ** 2
plt.imshow(gris_carre, cmap="gray")
```

5. Pareil, mais cette fois utilisez la racine carrée; quel effet cette fois ?

```{code-cell} ipython3
# votre code
gris_racine = np.sqrt(gris_moyenne)
plt.imshow(gris_racine, cmap="gray")
```

6. Convertissez l'image (de 3.a toujours) en type entier, et affichez la

```{code-cell} ipython3
# votre code
gris_entier = (gris_moyenne * 255).astype(np.uint8)
plt.imshow(gris_entier, cmap="gray")
```

## affichage grille de figures

+++

`````{admonition} Mettre plusieurs figures dans une grille

Mettons l'exercice sur pause pour l'instant, et voyons comment avec matplotlib, on peut mettre faire une figure composite, i.e. qui contienne plusieurs figures disposés dans une grille

**0) on dessine toujours la même chose**, juste avec des couleurs différentes

```python
# pas important, juste un exemple de truc à dessiner

X = np.linspace(0, 2*np.pi, 50)
Y = np.sin(X)
```

**1) on créé une figure globale et des sous-figures**

les sous-figures sont appelées `axes` par convention `matplotlib`  
on construit notre grille ici de 2 lignes et 3 colonnes

```python
# ici axes va être un tableau numpy de shape .. (2, 3)
fig, axes = plt.subplots(2, 3)
```

les cases pour les sous-figures sont ici dans la variable `axes`  
qui est un `numpy.ndarray` de taille 2 lignes et 3 colonnes

**2) on affiche des sous-figure dans des cases de la grille**

```python
# en haut à gauche
axes[0, 0].plot(X, Y, 'b')
axes[0, 1].plot(X, Y, 'r')
# en haut à droite
axes[0, 2].plot(X, Y, 'y')
axes[1, 0].plot(X, Y, 'k')
axes[1, 1].plot(X, Y, 'g')
axes[1, 2].plot(X, Y, 'm')
```

````{admonition} 3) cosmétique (optionnel)
:class: dropdown tip
on peut faire un peu de cosmétique, mais je vous mets en garde: 
quand on commence on ne s'arrête plus et on perd beaucoup de temps; 
préférez au début des affichages minimalistes à peu près lisibles
```python
fig.suptitle("sinus en couleur", fontsize=20) # titre général
axes[0, 0].set_title('sinus bleu')            # titre d'une sous-figure
axes[0, 2].set_xlabel('de 0 à 2 pi')          # label des abscisses
axes[1, 1].set_ylabel('de -1 à 1')            # label d'ordonnées
axes[1, 2].set_title('sinus magenta')
plt.tight_layout()                            # ajustement automatique des paddings
```
````
`````

```{code-cell} ipython3
# ce qui nous donne, mis bout à bout
import numpy as np
import matplotlib.pyplot as plt

X = np.linspace(0, 2*np.pi, 50)
Y = np.sin(X)

# le code
fig, axes = plt.subplots(2, 3)
print(f"{type(axes)=}")
print(f"{axes.shape=}")

axes[0, 0].plot(X, Y, 'b')
# axes[0, 1].plot(X, Y, 'r')
axes[0, 2].plot(X, Y, 'y')
axes[1, 0].plot(X, Y, 'k')
axes[1, 1].plot(X, Y, 'g')
axes[1, 2].plot(X, Y, 'm')

fig.suptitle("sinus en couleur", fontsize=20)
axes[0, 0].set_title('sinus bleu')
axes[0, 2].set_xlabel('de 0 à 2 pi')
axes[1, 1].set_ylabel('de -1 à 1')
axes[1, 2].set_title('sinus magenta')
plt.tight_layout();
```

## reprenons le TP

+++

**exercice**

Reprenez les trois images en niveau de gris que vous aviez produites ci-dessus:  
  A: celle obtenue avec la moyenne des rgb  
  B: celle obtenue avec la correction Y  
  C: celle obtenue avec la racine carrée

1. Affichez les trois images côte à côte
   ```text
   A B C
   ```

```{code-cell} ipython3
# votre code
A = gris_moyenne
B = gris_Y
C = gris_racine

fig, axes = plt.subplots(1, 3)
axes[0].imshow(A, cmap="gray")
axes[1].imshow(B, cmap="gray")
axes[2].imshow(C, cmap="gray")
plt.tight_layout()
```

2. Affichez-les en damier:
   ```text
   A B C
   C A B
   B C A
   ```

```{code-cell} ipython3
# votre code
grille = [
    [A, B, C],
    [C, A, B],
    [B, C, A],
]

fig, axes = plt.subplots(3, 3)
for i in range(3):
    for j in range(3):
        axes[i, j].imshow(grille[i][j], cmap="gray")
plt.tight_layout()
```

```{code-cell} ipython3

```
