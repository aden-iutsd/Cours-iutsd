# Cours

## Normalisation

### Rappels

#### Première forme normale

> Une relation R est dite en première forme normale ssi tous ses attributs sont atomiques.

**Exemple** :

| Livre | Titre | Auteur | Prix |
|-------|-------|--------| -----|
|       |Germinal|Pascal|10-12|
|       |Optimisation|Ali|13-14|
|       |Bases de données | Georges|12

La relation Livre n'est pas en première forme normale (1NF) car l'attribut prix n'est pas atomique.
La relation Livre peut être normalisée pour respecter la 1NF de la manière suivante :
| Livre | Titre | Auteur | PrixMin | PrixMax
|-------|-------|--------| -----|---
|       |Germinal|Pascal|10|12
|       |Optimisation|Ali|13|14
|       |Bases de données | Georges|12 | 12

#### Deuxième forme normale

> Une relation R est 2NF ssi :
> * Elle est en 1NF
> * Aucun attribut non-clé ne dépend d'une partie de la clé. 

**Remarque** : Toute relation dont la clé est composée d(un seul attribut et qui respecte la 1NF est forcément 2NF.

**Exemple 1** : 
$R(A,B,C,D) : \text{une relation } \{A \rightarrow B, C \rightarrow D, F \rightarrow C, FD \rightarrow A \}$, ensemble de dépendances fonctionnelles valides sur $R$.

**Graphe des DF de R**

```mermaid
graph LR
A((A)) -->B((B))
F((F)) --> A
D((D)) --> A
C((C)) --> D
F --> C
```

Tout attribut n'appartenant à aucune des parties droites des dépendances fonctionnelles doit faire partie de la clé. ⇒ $F \in \text{clé}$
$$\begin{aligned} FD \rightarrow A   &(1) \\ F \rightarrow C &(2) \\ C \rightarrow D &(3) \\ F \rightarrow D &(4)\end{aligned}$$

$$\begin{aligned} (4) \text{ et } (1) \ &FF \rightarrow A \\ &F \rightarrow A \\ &F \rightarrow B \text{ par transitivité}\end{aligned}$$ $F$ est une clé candidate, elle est unique.
⇒ Clé atomique : $R$ 2NF.

**Exemple 2** :

$R(ABCD) : A \rightarrow B, C\rightarrow D.$
$(A) \rightarrow (B)$ : une partie de la clé ⇒ un attribut n'appartenant pas à la clé.
$(C) \rightarrow (D)$

Clé de $R$ :
$AC$ doivent faire partie de la clé.
$AC \rightarrow A, AC\rightarrow C, AC \rightarrow B, AC \rightarrow D$ : $AC$ est une clé unique.
$R$ n'est pas 2NF car $A \rightarrow B$ est de la forme $\text{partie de la clé } \rightarrow \text{ attribut} \notin \text{ clé}$.

CF (1)

Cette décomposition est sans perte de dépendances fonctionnelles ?
Oui : les deux DF initiales sont préservées.

| R | A | B | C | D
|-------|-------|--------| -----|---
|       |1|2|3|4
|  $\rightarrow$     |1|2|7|8
|       |9 | 10|3 | 4

$$ \\ $$
|R1|A|B|
--|--|--
|  | 1 | 2 |
|  | 9 | 10 |

$$ \\ $$
|R2|C|D|
--|--|--
|  | 3 | 4 |
|  | 7 | 8 |

$$  R1 (\times) R2 = R1 \times R2 $$ $$ \begin{aligned}R1 (\times) R2 &= R :\text{sans perte d'informations}  \\ &\neq R : \text {perte d'informations} \end{aligned} $$ $R1 (\times) R2 :$

|A|B|C|D|
|-|-|-|-|
|1|2|3|4|
|1|2|7|8|
|9|10|3|4|
|$\bf9$|$\bf10$|$\bf7$|$\bf8$|

Le tuple $(9,10,7,8) \notin R$.
$$ R1 (\times) R2 = R + (9, 10, 7, 8)$$

**Exemple 3**

$$ R(A,B,C,D) : AB \rightarrow C, DE \rightarrow C, B \rightarrow E, E \rightarrow B$$

```mermaid
graph LR
A((A)) -->C((C))
B((B)) --> C
B --> E((E))
E --> B
E --> C
D((D)) --> C
```
$$ C \notin \text{clé} \\ D \in \text{clé} \\ A \in \text{clé} \\ B \rightarrow E  $$ $$ \begin{aligned} ABD &\rightarrow A \text{ oui} \\ & \rightarrow B \text{ oui} \\ &\rightarrow C \\ &\rightarrow D \text{ oui} \\ &\rightarrow E \text{ oui} \end{aligned}$$ $A,B,D$ clé candidate n°1, $A,D,E$ clé candidate n°2.
Choix de clé : $A,D,E$. $$C \notin \text { clé}, B \notin \text{ clé}$$ $$ADE \rightarrow B \\ ADE \rightarrow C$$
$E \rightarrow B$ & $DE \rightarrow C$ sont deux dépendances fonctionnelles qui font que $R$ n'est pas 2NF.

CF (2)

#### Troisième forme normale

> Une relation est dite 3NF ssi :
> * Elle est 2NF,
> * Il n'existe aucune dépendance fonctionnelle entre deux attributs non-clé.

**Exemple** : 
$R(A,B,C,D) : A \rightarrow B, B \rightarrow C, C \rightarrow D, D \rightarrow E, E \rightarrow A$

```mermaid
graph LR
A((A)) -->B((B))
B --> C((C))
C --> D((D))
D --> E((E))
E --> A
```
$A,B,C,D,E$ sont toutes clés candidates.
On choisit $A$ comme clé.
$R$ est 2NF car clé atomique composée d'une seul attribut.
$R$ n'est pas 3NF car :
$B \rightarrow C$ de la forme $\text{attribut} \notin \text{clé} \rightarrow \text{attribut} \notin \text{ clé}$ 
$C \rightarrow D$ de la forme $\text{attribut} \notin \text{clé} \rightarrow \text{attribut} \notin \text{ clé}$  
$D \rightarrow E$ de la forme $\text{attribut} \notin \text{clé} \rightarrow \text{attribut} \notin \text{ clé}$  

Décomposition : 
CF (3)

$$ R = R1 \underset{B}{(\times)}  R2 \underset{C}{(\times)} R3 \underset{D}{(\times)} R4 \underset{E}{(\times)} R5$$

#### Forme normale de Boyce-Codd

> Une relation $R$ est dite BCNF ssi toutes ses DF sont de la forme $\text{clé} \rightarrow \text{attribut}$.

**Exemple** : ${\bf \text{Localisation}} (\text{CRU}, \text{Pays}, \text{Région}, \text{Qualité}$) 

$\text{CRU}, \text{Pays} \rightarrow \text{Région}$
$\text{CRU}, \text{Pays} \rightarrow \text{Qualité}$
$\text{Région} \rightarrow \text{Pays}$

|Localisation|CRU|Pays|Région|Qualité
|-|-|-|-|-|
||Chenas|France|Beaujolais|Excellent|
||Juliénas|France|Beaujolais|Excellent|
||Morgan|France|Beaujolais|Bon|
||Chablis|USA|Californie|Moyen|
||Brouilly|USA|Californie|Médiocre|
||Chablis|France|Beaujolais|Excellent|

Considérons $(\text{CRU, Pays)}$ clé de la relation. Cette relation est 3NF mais n'est pas BCNF car $\text{Région} \rightarrow \text{Pays}$ n'est pas de la forme $\text{clé} \rightarrow \text{attribut}$.

CF4

### Quatrième forme normale

La BCNF n'étant pas suffisante pour éliminer les redondance, on introduit d'autres types de formes normales.

#### Dépendances Multivaluées

On considère la relation $\text{Étudiant}(\text{NE}, \text{Cours}, \text{Sport})$.
Exemple de contenu de cette relation :
|$\text{NE}$|$\text{Cours}$|$\text{Sport}$|
|-|-|-|
|100|BD|Tennis|
|100|BD|Foot|
|100|Réseau|Tennis|
|100|Réseau|Foot|
|200|Réseau|Vélo|

La clé de cette relation est constituée de l'ensemble des attributs $\text{NE}$, $\text{Cours}$ et $\text{Sport}$. Cette relation ne possède aucune dépendance fonctionnelle et elle respecte les formes normales BNCF et 3NF.

$R(A_1, \dots, A_n) \ ;X \subseteq \{A_1, \dots, A_n \} \ ; Y \subseteq \{A_1, \dots, A_n \} \ ; X \neq Y.$
$X$ multidétermine $Y$ ($X \rightarrow \rightarrow Y$) ssi $\exists \ Z \in R$ avec $Z \neq Y$, $Z \neq X$ et $Z = R - X - Y$.
C'est à dire $X \rightarrow \rightarrow Y \iff (x, y, z) \in R$ et $(x', y', z') \in R$. D'où $(xy'z) \in R$ et $(xyz') \in R$.

Les dépendances fonctionnelles sont des cas particulier des dépendances multivaluées.

**Exemple 1** : Considérons la relation $\text{Vol}(\text{NV}, \text{Avion}, \text{Pilote})$.
* Tout pilote est appelé à piloter tout avion sur n'importe quel vol,
* Les avions et les pilotes sont indépendants.

|$\text{Vol}$|$\text{NV}$|$\text{Avion}$|$\text{Pilote}$|
|-|-|-|-|
| |2|A380|115|
| |3|B737|115|
| |5|A360|115|
|$\rightarrow$ |2|B737|120|
| |2|B737|115|
| |2|A380|120|
| |3|B740|120|
| |3|B740|115|
| |3|B737|120|

$$\text{NV} \rightarrow \rightarrow \text{Pilote}$$ $$\begin{array}{ccc} 2 & \text{A380} & 115 \\ 
x & y & z \\ \\
2 & \text{B737} & 120 \\
x & y' & z'
\end{array} \implies \begin{array}{cccc} x & y' & z & \in R \\ 
2 & \text{B737} & 115 & \\ \\
x & y & z' & \in R \\
2 & \text{A380} & 120 &
\end{array}$$
$$\begin{aligned} R(A,B,C) : &A \text{ indépendant de }  C \\ 
&B \rightarrow \rightarrow A \\
&B \rightarrow \rightarrow C
\end{aligned} $$

**Exemple 2** : $\text{Personne}(\text{N°SS}, \text{N°Véhicule}, \text{Prénom-Enfant})$, $\text{N°Véhicule}$ et $\text{Prénom-Enfant}$ indépendants : $\begin{array}{l}\text{N°SS} \rightarrow \rightarrow \text{N°Véhicule} \\ \text{N°SS} \rightarrow \rightarrow \text{Prénom-Enfant} \end{array}$.

**Propriétés** : 
* Augmentation : $X \rightarrow \rightarrow Y \implies XV \rightarrow \rightarrow YV$
* Transitivité : $\begin{array}{l} X \rightarrow \rightarrow Y \\ Y \rightarrow \rightarrow Z \end{array} \implies X \rightarrow \rightarrow Z$
* Complémentation : $X \rightarrow \rightarrow Y \implies X \rightarrow \rightarrow R - X - Y$




### Cinquième forme normale

## Gestion des transactions

## Optimisation

## Techniques de stockage

## Administration

# Exercices

## Exercice 1 

>Soit la relation :

|Personne|Nom|Profession|
|-|-|-|
||Martin|Président|
||Arij|Enseignante, chercheuse|
||Anne|Enseignante|
>Quelle est la forme normale de cette relation ?
> > La relation n'est pas normalisée.

> Même question pour  $\text{Fournisseur}(\text{Nom},\text{Adresse},\text{Article},\text{Prix})$, $\text{Nom} \rightarrow \text{Adresse}$.
> > Les attributs de cette relation sont atomiques : 1NF. Clé : $(\text{Nom Article Prix})$

> Même question pour $\text{Vin}(\text{Cru}, \text{Type}, \text{Client}, \text{Remise})$, $\text{Cru} \rightarrow \text{Type}$.
> > Les attributs sont atomiques : 1NF. Clé : $(\text{Cru} \ \text{Client} \ \text{Remise})$.

> Même question pour $\text{Voiture}(\text{Marque}, \text{Type}, \text{Puissance}, \text{Couleur})$, $\text{Marque}, \text{Type} \rightarrow \text{Puissance}$ ; $\text{Puissance} \rightarrow \text{Couleur}$.
> > Les attributs sont atomiques et aucun attribut non-clé ne dépend d'une partie de la clé : 2NF.
> <div align="center"><img src="https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/UML%20Voiture.png"></img>

> Même question pour $\text{Cru}, \text{Type}, \text{Client} \rightarrow \text{Remise}$, $\text{Remise} \rightarrow \text{Cru}$.
```mermaid
graph LR
A((Cru)) --> B{/}
C((Type)) --> B
D((Client)) --> B
B --> E((Remise))
E --> A 
```
> > $\text{Cru}, \text{Type}, \text{Client}$ : 2NF
> > $\text{Remise}, \text{Type}, \text{Client}$ : 1NF


|R|A|B|C|
|-|-|-|-|
||1|2|4|
||1|3|4|
||2|5|7|
||1|2|7|
||1|3|7|
||1|5|4|
||1|5|7|

> Quelles sont les dépendance fonctionnelles ou multivaluées de $R$ ?
> > $A \rightarrow B$ (non valide),
> > $A \rightarrow C$ (non valide),
> > $B \rightarrow A$ (non valide),
> > $B \rightarrow C$ (non valide),
> > $AB \rightarrow C$ (non valide),
> > $AC \rightarrow B$ (non valide),
> > $BC \rightarrow A$ (non valide),
> > 
> > $$ \begin{array}{ccc|ccc} 
 x & y & z & & & \\
 1 & 2 & 4 & 1 & 3 & 4 \\
 1 & 3 & 7 & 1 & 2 & 7 \\
 x' & y' & z' & & &
\end{array}$$  $$ \begin{array}{ccc|ccc} 
 1 & 3 & 4 & 1 & 5 & 4 \\
 1 & 5 & 7 & 1 & 3 & 7 \\
\end{array}$$ $$ \begin{array}{ccc|ccc} 
 1 & 2 & 7 & 1 & 2 & 4 \\
 1 & 3 & 4 & 1 & 3 & 7 \\
\end{array}$$ $$ \begin{array}{ccc|ccc} 
 1 & 5 & 4 & 1 & 5 & 7 \\
 1 & 3 & 7 & 1 & 3 & 4 \\
\end{array}$$ $$ A \rightarrow \rightarrow B $$ $$A \rightarrow \rightarrow C$$
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNjAyNTg4MjQsMTkxODUzOTA1MCw2Mj
k5MTQzMTIsMjY0OTUxMjI2LDE2Njc0MDIwMzMsODIzNzUyNzIx
LDc0ODYwMDMyNF19
-->