# Cours

> M. Ammi (ammi@limsi.fr) Bureau A164

## Pipeline Graphique

Entrée : Modèle géométrique, d'illumination, de caméra et de fenêtre.
Sortie : Couleur et intensité de l'affichage.

### Transformations de modélisation

Projections et rotations qui permettent de positionner des objets dans la scène.
⇒ Repère objet

### Illumination (Shading)

Interaction de diffusion de la lumière avec un objet en fonction des propriétés de celui-ci.

### Transformations d'affichage

Représentation des objets dans le référentiel de la caméra au moyen de projections (changement de référentiel).
⇒ Repère scène

### Clipping

Élimination de tout ce qui n'est pas visible / utile par la caméra (trop loin, derrière, etc) du calcul d'affichage.
⇒ Repère caméra

### Transformations écran (Projection)

Passage de la 3D à la 2D — Introduction de la perspective, de la stéréoscopie, des ombres portées et des textures.
⇒ Repère caméra normalisé (NDC)

### Pixellisation (Rasterization)

Dessin des primitives en 2D (activation des suites de pixels).
⇒ Espace écran

### Visibilité / Affichage

Élimination des faces cachées.

## Implémentation

Le hardware s'est progressivement introduit dans la pipeline (absent dans un premier temps, puis limité à l'affichage à la pixellisation et à l'affichage, avant de se généraliser), est devenu configurable puis programmable.

## Concepts mathématiques

### Entités & opérations

**Scalaire** : grandeur définie par un nombre et une unité.
**Vecteur** : entité mathématique définie par n valeurs numériques extraites d'un même ensemble (par exemple $\mathbb N, \mathbb Z, \mathbb R, \mathbb C$).
Dans un repère, les vecteurs sont placés dans une base.
Signe du produit scalaire : 
 - $\vec A \cdot \vec B \gt 0 : \ -90° \lt \theta \lt 90°$
 -  $\vec A  \vec B = 0 : \ \pm 90°$
 - $\vec A \cdot \vec B \lt 0 : \ -180° \lt \theta \lt -90° \text{ ou } 90° \lt \theta \lt 180°$

Produit vectoriel :
$$ \vec A \cdot \vec B = \left |
\begin{matrix}
\vec i & \vec j & \vec k \\
A_x & A_y & A_z \\ 
B_x & B_y & B_z
\end{matrix} \right | = + \vec i \left |
\begin{matrix}
\vec A_y & \vec A_z \\
\vec B_y & \vec B_z
\end{matrix} \right | - \vec j \left | \begin{matrix}
\vec A_x & \vec A_z \\
\vec B_x & \vec B_z
\end{matrix} \right | + \vec k \left |
\begin{matrix}
\vec A_x & \vec A_y \\
\vec B_x & \vec B_y
\end{matrix} \right |
$$
Règle de la main droite :
![](https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/main%20droite.png)
### Transformations

#### Rigide / Euclidienne

⇒ Translation (3 DDL) + Rotation (3 DDL) = 6 degrés de liberté

#### Similitude

⇒ Translation + Rotation + Homothétie (3 DDL) : 9 degrés de liberté

#### Transformation Affine

⇒ Translation + Rotation + Homothétie + Réflexion (3DDL) : 12 degrés de liberté

#### Transformation Projective ou Homographie

⇒ Translation + Rotation + Homothétie + Réflexion + Projection (3DDL) : 15 degrés de liberté

### Tracé de segments

Lorsqu'on trace une droite sur un nombre discret de pixels, si la pente est < 1, la discrétisation se fait selon $x$, si elle est > 1, elle se fait selon $y$. 

#### Algorithmes de Brensenham

Pour chaque pixel, on calcule de manière incrémentale la distance entre la pseudo-distance du centre du pixel à la droite.
Pour calculer e, on utilise le théorème de Thales : ${{dy} \over {dx}} = {{e} \over {1}} \implies e = {dy \over dx}$.
$$ a = {dy \over dx} $$  Pour chaque itération : $$e = {e + a}, \  {x = x + 1}  \\ \text{ si } e > 0,5 : y = y + 1, \ e = e - 1$$


### Tracé de cercles

L'algorithme fonctionne de la même manière que le tracé de segments. On procède par des arcs correspondant à un huitième de cercle, puis en appliquant des symétries (CF diapo). 

# TD

## TD0

### Exercice 1
<div align="center"><img src="https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/cg_exo1.png" width=500px /></div>


Équation du plan : $ax + by + cz + d = 0$
Vecteur orthogonal : $\vec v \cdot \vec d = 0$
$$
\begin{aligned}\vec v = \begin{pmatrix} x \\ y \\ z \end{pmatrix} - \vec p = \begin{pmatrix} x - p_x \\ y - p_y \\ z - p_z \end{pmatrix} &\iff
\vec v \cdot \vec d = \begin{pmatrix} x - p_x \\ y - p_y \\ z - p_z \end{pmatrix} \cdot \begin{pmatrix} d_x \\ d_y \\d_z \end{pmatrix} \\ \\
& \iff (x - p_x) d_x + (y - p_y) d_y + (z - p_z) d_z = 0 \\ \\
& \iff{d_x \over a} x + {d_y \over b} y + {d_z \over c}z - {(p_x d_x + p_y d_y + p_z d_z) \over d} = 0
\end{aligned}
$$

### Exercice 2

Le vecteur normal à l'une des faces doit être orthogonal à au moins deux des arêtes de l'autre face pour que les deux faces soient parallèles.

<div align="center"><img src="https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/triangles.png" width=200px/></div>

### Exercice 3

$$ F_1 : \vec{n_1} = \vec{AD} \land \vec{AB} = \overset{\vec{AD}}{\begin{pmatrix} 0 \\ 1 \\ -1 \end{pmatrix}}  \land \overset{\vec {AB} }{\begin{pmatrix} 1 \\ 0 \\ -2 \end{pmatrix}} = \begin{pmatrix} 2 \cdot (-2) - (-1) \cdot (0) \\ -(0 \cdot (-2) - (-1) \cdot 1) \\ 0 \cdot{ 0}  - (2) \cdot (1)\end{pmatrix} = \begin{pmatrix} -4 \\ -1\\ 2\end{pmatrix} $$

$$ F_2 : \vec{n_2} = \vec{AC} \land \vec{AD} = \overset{\vec{ AC}}{\begin{pmatrix} -1 \\ 0 \\ -2 \end{pmatrix}}  \land \overset{\vec{AD}}{\begin{pmatrix} 0 \\ 2 \\ -1 \end{pmatrix}} = \begin{pmatrix} 0 \cdot (-1) - (-2) \cdot 2 \\ -(-1 \cdot (-1) - (-2) \cdot 0)) \\ -1 \cdot{ 2} - 0  \cdot 0\end{pmatrix} = \begin{pmatrix} 4 \\ -1\\ -2\end{pmatrix}$$

$$ F_3 : \vec{n_3} = \vec{DC} \land \vec{DB} = \overset{\vec{ DC}}{\begin{pmatrix} -1 \\ -2 \\ -1 \end{pmatrix}}  \land \overset{\vec{DB}}{\begin{pmatrix} 1 \\ -2 \\ -1 \end{pmatrix}} = \begin{pmatrix} -2 \cdot (-1) - (-1) \cdot (-2) \\ -1 \cdot (-1) - (-1) \cdot 1 \\ -1 \cdot{ (-2)} - (-2)  \cdot 1\end{pmatrix}= \begin{pmatrix} 0 \\ 0\\ 4\end{pmatrix} $$

$$ F_4 : \vec{n_4} = \vec{AB} \land \vec{AC} = \overset{\vec{ AB}}{\begin{pmatrix} 1 \\ 0 \\ -2 \end{pmatrix}}  \land \overset{\vec{AC}}{\begin{pmatrix} -1 \\ 0 \\ -2 \end{pmatrix}} = \begin{pmatrix} 0 \cdot (-2) - (-2) \cdot 0 \\ (-1 \cdot (-2) - (-2) \cdot (-1)) \\ -1 \cdot{ 0} - 0  \cdot (-1)\end{pmatrix}= \begin{pmatrix} 0 \\ 4\\ 0\end{pmatrix} $$

Pour être visible, le produit scalaire doit être positif.

$$ \Delta = \begin{pmatrix} 0 \\ 0 \\ -1 \end{pmatrix} $$
$$ \vec{n_1} \cdot \vec{\Delta} = \begin{pmatrix} -4 \\ -1 \\ -2 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 0 \\ -1 \end{pmatrix} = (-4 \cdot 0) + (-1 \cdot 0) + (-2 \cdot -1) = 2 \implies \text{face visible} $$
$$ \vec{n_2} \cdot \vec{\Delta} = \begin{pmatrix} 4 \\ -1 \\ -2 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 0 \\ -1 \end{pmatrix} = (4 \cdot 0) + (-1 \cdot 0) + (-2 \cdot -1) = 2 \implies \text{face visible} $$
$$ \vec{n_3} \cdot \vec{\Delta} = \begin{pmatrix} 0 \\ 0 \\ 4 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 0 \\ -1 \end{pmatrix} = (0 + 0 -4) = -4 \implies \text{face non visible} $$
$$ \vec{n_4} \cdot \vec{\Delta} = \begin{pmatrix} 0 \\ 4 \\ 0 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 0 \\ -1 \end{pmatrix} = (0 + 0 + 0) = 0 \implies \text{face parallele à l'axe de vue} $$
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTM4NzYzMzcsLTY5MjY4NzM1NywtOT
kzNzE1NDIyLC0yNDYyODQ5MTQsMTMwMjY2MDM4MCwtMTQ3ODA2
NzE2MCwtMTk3NjgyOTg3NCw1MzQxODU5OTQsLTIwNzAyNjg5NT
AsLTMxNjk3NDc4MSwtMTU3OTM0MDQwMiwxNDY3MzQ2NTYzLDEy
Mjc5NDI3NzEsLTEwNzEzMTg5NjEsMTEzODY2MjcyOF19
-->