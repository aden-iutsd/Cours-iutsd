https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/transition.png# Langages : interprétation et compilation

> Pablo Rauzy pr@up8.edu pablo.rauzy.name/teaching/liec

**Interpréteur** : programme qui exécute directement les instructions du code source.
 * Lit et analyse une expression,
 * L'évalue si elle est valide
 * (CF diapo)

**Compilateur** : programme qui traduit un langage dans un autre langage plus bas niveau.
Le compilateur produit généralement un exécutable mais pas nécessairement. 
Le compilateur est *correct* si la sémantique du code produit est la même que celle du code source.

Il se divise en trois parties : 
 * **front end** : lecture du code source et transformation en langage intermédiaire.
 * **middle end** : fait plusieurs passes de traitement dans le langage intermédiaire.
 * **back end** : produit le code final à partir du langage intermédiaire.

**Lexer** :

![](https://raw.githubusercontent.com/LifelongNovember/Cours/master/img/lexer.svg?sanitize=true)
parser - appels récursifs : `(begin`
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTYyNTQ3MjVdfQ==
-->