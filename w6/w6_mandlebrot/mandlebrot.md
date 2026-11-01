---
jupytext:
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

# l'ensemble de Mandelbrot

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl
```

```{code-cell} ipython3
# à vous de jouer

def mandelbrot(h, w):
    max = 20

    re = np.linspace(-2, 0.8, w)
    im = np.linspace(-1.4, 1.4, h)
    X, Y = np.meshgrid(re, im)

    c = X + 1j * Y
    z = c.copy()

    diverge = np.full((h, w), max)

    for n in range(max):
        masque = (np.abs(z) > 2) & (diverge == max)
        diverge[masque] = n

        z = z**2 + c

        z[diverge < max] = 0

    plt.figure()
    plt.imshow(diverge)
```

```{code-cell} ipython3


mandelbrot(1000, 1000)
```

```{code-cell} ipython3
plt.figure()
print(plt.gcf().canvas.get_supported_filetypes())
```

```{code-cell} ipython3


def mandelbrot2(x_min, x_max, y_min, y_max, max=20, h=1000, w=1000, fichier=None):
    re = np.linspace(x_min, x_max, w)
    im = np.linspace(y_min, y_max, h)
    X, Y = np.meshgrid(re, im)

    c = X + 1j * Y
    z = c.copy()
    diverge = np.full((h, w), max)

    for n in range(max):
        masque = (np.abs(z) > 2) & (diverge == max)
        diverge[masque] = n
        z = z**2 + c
        z[diverge < max] = 0

    plt.figure()
    # extent : les axes montrent les vraies coordonnées ; origin="lower" : les imaginaires croissent vers le haut
    plt.imshow(diverge, extent=(x_min, x_max, y_min, y_max), origin="lower")

    # si on donne un nom de fichier, on sauve la figure (le format vient de l'extension)
    if fichier is not None:
        plt.savefig(fichier)
```

```{code-cell} ipython3
# test #1 : l'ensemble en entier, sauvé dans un fichier svg
mandelbrot2(-2, 0.8, -1.4, 1.4, fichier="mandelbrot.svg")
```

```{code-cell} ipython3
# test #2 : un zoom sur le bord de l'ensemble, avec plus d'itérations
mandelbrot2(-0.8, -0.7, 0.05, 0.15, max=100)
```

```{code-cell} ipython3
# test #3 : un autre zoom, en haut de l'ensemble
mandelbrot2(-0.2, 0.2, 0.6, 1.0, max=60)
```

***

```{code-cell} ipython3

```
