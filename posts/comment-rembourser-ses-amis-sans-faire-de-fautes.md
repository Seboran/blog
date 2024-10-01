---
title: Comment j'ai recréé tricount sans demander des millions à la banque populaire ?
date: 2024-10-01
author: Nirina Rabeson
---

Imaginez, vous rentrez de vacances avec vos amis. Imaginons que vos ami·es s'appellent Alice, Bob, Charlie et Eve. Avant de commencer à vous remémorer vos moments, vous vous demandez "hey, combien je dois rembourser à mes ami·es ?".

Vous savez qu'Alice a dépensé 333€ pour tout le monde, Bob 213€, Charlie n'a rien dépensé, Eve a dépensé 10€ (elle a payé la chocolatine), et vous-même avez dépense 50€. La question se pose : comment rembourser tous vos amis ? **Comment rembourser de façon optimale vos amis ?**

Il se trouve que la réponse est plus difficile à obtenir qu'il n'y paraît !!! Embarquez dans le merveilleux monde qu'est l'optimisation mathématique, découvrons rapidement le concept de la résolution de problèmes, et voyons comment on peut réaliser le tout avec un tout petit peu de Python et un peu d'aide d'une amie.

---

## Une intuition du problème

Retenez pour plus tard : Alice a dépensé 333€, Bob 213€, Charlie 0€, Eve 10€, et vous 50€. Nous pouvez écrire tout cela dans un tableau qui nous sera très important pour plus tard :

|Nom|Total dépensé|
|-|-|
|Alice|333|
|Bob|213|
|Charlie|0|
|Eve|10|
|Vous|50|


Comment commencer à résoudre ce problèlme ? Comment commencer à résoudre ce genre de problème ? Vous avez une idée ?

Il se trouve que le problème est trop compliqué à résoudre pour l'instant ! Simplifions le un tout petit peu. Considérons le même problème, mais avec seulement 3 personnes et les mêmes sommes. **Nous allons supposer qu'Eve et vous n'êtes pas parties en vacances**, et qu'ils n'ont rien dépensé. Voici son **tableau** :

|Nom|Total dépensé|
|-|-|
|Alice|333|
|Bob|213|
|Charlie|0|

Comment interpréter ce tableau ? Commençons par la dépense d'Alice : elle a dépensé 333€ pour elle-même, Bob et Charlie, on peut supposer qu'il faut diviser l'addition entre ces 3 personnes. Supposons que nous avons un registre dans lequel nous notons ce que tout le monde doit à tout le monde. Nous avons alors : Charlie qui doit 333€/3 = 111€, Bob qui doit 111, et Alice à qui on doit au total 333€ - 333€/3 = 222€. Pour calculer ce nombre, on prend la part totale qu'elle a dépensé, et on lui retire la partie qui lui revient.

Si on tient notre registre de dépenses, il se note ainsi :

|Alice|Bob|Charlie|
|---|---|---|
|222€|-111€|-111€|

Le signe moins représente l'argent que doivent Bob et Charlie, et la somme positive représente ce qu'Alice attend.

On remarque deux choses : la première, c'est que le total de la ligne vaut 0€. La deuxième, c'est qu'on sait comment rembourser Alice : Si Bob envoie 111€ à Alice, et Charlie 111€ à Bob, alors les trois personnes seront quittes envers Alice.

Voyons maintenant comment rembourser Bob ! Faisons la même gymnastique avant : supposons qu'Alice et Charlie doivent 213€/3 = 71€ à Bob, et que Bob doit recevoir en tout 213€ - 213€/3 = 142€, nous pouvons représenter le registre de la façon suivante :

|Alice|Bob|Charlie|
|---|---|---|
|-71|142€|-71€|

Pour que Alice et Charlie soient en bons comptes avec Bob, iels doivent envoyer 71€ à Bob.

Notre première solution de remboursement est donc la suivante :

- Bob doit 111€ à Alice
- Charlie doit 111€ à Alice
- Alice doit 71€ à Bob
- Charlie doit 71€ à Bob

En la réécrivant on peut la simplifie pour qu'Alice ne donne pas d'argent à Bob :

- Bob doit 40€ à Alice
- Charlie doit 111€ à Alice
- Charlie doit 71€ à Bob

::: info
Les très attentifs ou très malins me diront : "Mais Nirina, j'ai un algorithme bien plus simple ! Tu prends la personne à qui tu dois le plus, tu prends la personne qui doit le plus d'argent, tu lui fais rembourser un maximum d'argent, puis tu recommences avec à qui on doit le plus d'argent et qui en doit le plus ?"

C'est une excellente remarque, mais lis la suite de l'article et tu verras pourquoi je rejette ta proposition ?
:::

## Un début d'optimisation

Bob fait la gueule. Il pense qu'il n'a pas besoin de rembourser Alice, et que Charlie doit tellement d'argent qu'il n'a rien à avancer. Il refuse la solution et en demande une plus optimale ! Soit ! Comment la rendre plus optimale ? Au hasard, **faisons l'addition de chacun des registres.**

||Alice|Bob|Charlie|
|---|---|---|---|
||222€|-111€|-111€|
||-71€|142€|-71€|
|||||
|**Total**|151€|31€|-182€|

Comment interpréter ce total ? Comme précédemment, nous savons qu'en tout, Alice doit recevoir 151€, que Bob doit en recevoir 31€, et que Charlie doit dépenser 182€, mais maintenant comment répartir tout cet argent ? C'est le moment d'introduire une citation d'Albert Einstein

> Pour résoudre un problème, il faut sortir du cadre dans lequel il s'est créé. 😎

Merci pélo pour le conseil. Mais comment le concrétiser ??? Rappelons-nous ce qu'on veut trouver : combien chacun doit à chacun. Nous savons que Bob doit de l'argent à Alice, et que Charlie doit de l'argent à Alice et Bob. Faisons un nouveau tableau, mais un peu différent : écrivons toutes les façons possibles pour notre trio de se rembourser de l'argent.

||Alice|Bob|Charlie|
|-|-|-|-|
|Alice||L'argent que Bob doit à Alice|L'argent que Charlie doit à Alice|
|Bob|L'argent que Bob reçoit d'Alice||L'argent que Charlie doit à Bob|
|Charlie|L'argent que Charlie reçoit d'Alice|L'argent que Charlie reçoit de Bob||

Ah ha ! Nous avons un nouveau cadre, nous sommes passés de registre d'une ligne à un tableau en deux dimensions ! Peut-être une autre façon de résoudre le problème ?

Quelques observations :

- Alice ne peut pas devoir de l'argent à elle-même, d'où les cases vides à la diagonale
- L'argent que Bob doit à Alice, c'est l'opposé de l'argent que Bob reçoit d'Alice
- Nous pouvons écrire un nombre pour chacune de ces quantités

Qu'est-ce que cela donne ?

Écrivons `X` l'argent que Bob doit à Alice, `Y` l'argent que Charlie doit à Alice, et `Z` l'argent que Charlie doit à Bob, et nous avons ce tableau :

||Alice|Bob|Charlie|
|-|-|-|-|
|Alice|0|`X`|`Y`|
|Bob|`-X`|0|`Z`|
|Charlie|`-Y`|`-Z`|0|

Si vous avez eu la chance de faire des mathématiques supérieures, vous remarquez quelque chose : nous avons écrit une matrice `antisymétrique`. Elle s'écrit ainsi :

$$
\begin{pmatrix}
0&X&Y\\
-X&0&Z\\
-Y&-Z&0\\
\end{pmatrix}
$$

## Au secours ! Des mathématiques !

Nous avons sorti les mathématiques. Mais vous n'avez pas besoin d'aide pour les comprendre. Elles sont là pour vous aider. Dites-vous juste qu'une matrice, c'est exactement le tableau que nous avons délicatement construits ensemble. Oubliez la juste quelques secondes, et revenons encore à notre tableau :

||Alice|Bob|Charlie|
|-|-|-|-|
|Alice|0|`X`|`Y`|
|Bob|`-X`|0|`Z`|
|Charlie|`-Y`|`-Z`|0|

Comment interpréter ce tableau ? Il faut comprendre : Alice reçoit `X` + `Y`€, Bob en reçoit `-X` + `Z`€, et Charlie "reçoit" `-Y` + `-Z`€.

Mais combient valent ces somems ? **Revenons au registre de toutes les dépenses** :

||Alice|Bob|Charlie|
|---|---|---|---|
||222€|-111€|-111€|
||-71€|142€|-71€|
|||||
|**Total**|151€|31€|-182€|

Nous savons qu'en tout, Alice doit recevoir 151€, Bob 31€, et Charlie -182€. Hors, Alice doit recevoir `X` + `Y`€, Bob `-X` + `Z`€, et Charlie `-Y` + `-Z`. Cela nous donne trois équations !!!

- `X` + `Y` = 151€
- `-X` + `Z` = 31€ 
- `-Y` + `-Z` = -182€

Et là, nous avons gagné : ceci est un système de 3 équations à 3 inconnues. Comment le résoudre ? [En demandant à Wolfram Alpha](https://www.wolframalpha.com/input?i=X+%2B+Y+%3D+151%2C+-X+%2B+Z+%3D+31%2C++-Y+%2B+-Z+%3D+-182)

La réponse que donne la machine est :

- `Y` = 151 - `X`
- `Z` = `X` + 31

Mais qu'est-ce que ça veut dire ??? Combien vaut `X` ? Pourquoi `Y` et `Z` dépendant de `X` ?

Pour faire simple : Cela signifie que la solution n'a pas une solution unique. En fait, plein de solutions existent. Et c'est tout à fait normal. La première réponse que nous avions fonctionnait, mais nous ne savions pas si elle était optimale :

- Bob doit 40€ à Alice
- Charlie doit 111€ à Alice
- Charlie doit 71€ à Bob

Si on revient à notre Matrice, cela signifie que :

- `X` = 40€
- `Y` = 111€
- `Z` = 71€

Est-ce notre solution marche ? Oui car en remplaçant dans nos équations les valeurs, nous avons 111 = 151 - 40 et 71 = 40 + 31 !!! C'est rassurant.

Mais est-ce la solution optimale ???

## Prouvons l'optimalité de notre solution

### Que veut dire optimal ?

Vous avez lu tout cet article jusqu'ici, et vous n'avez pas encore terminé. Mais c'est l'occasion d'une petite pause philosophique : **Est-il possible de représenter mathématiquement l'optimalité ?** Qu'est-ce que cela veut dire qu'une solution est optimale ? Il nous faut un critère pour dire qu'une solution de remboursement est optimale. Notre critère d'optimalité ici est simple : on veut pouvoir se donner le minimum d'argent que possible. Je ne veux pas devoir donner plus d'argent que nécessaire si c'est pour me le faire rembourser plus tard. mathématiquement, cela s'écrit simplement : nous voulons **minimiser** l'argent viré, donc la somme (absolute) des trois valeurs `X`, `Y` et `Z`. Mathématiquement, cela s'écrit : minimiser `|X|` + `|Y|` + `|Z|`

Est-ce que notre solution (40, 111, 71) est optimale ? Nous pouvons la calculer : 40 + 111 + 71 = 222. Est-ce que nous avons un meilleur résultat ?

Je prétends que la solution dans laquelle `X` = 0 est optimale. Testons : 

1. Calculer les valeurs de `Y` et `Z`. Dans notre cas, `Y` = 151 - 0 = 151 et `Z` = 0 + 31 = 31
2. Calculer le total de `X` + `Y` + `Z` = 0 + 151 + 31 = 182
3. 182 < 222 !!! Nous déduisons donc que la solution (0, 151, 31) est meilleure ! Qu'est-ce que cela veut dire ???

Cela veut dire que si Bob ne doit rien à Alice, que Charlie doit 151€ à Alice, et que Charlie doit 31€ à Bob, alors la solution est bien meilleure. Résumons :

- Bob ne doit rien à Alice
- Charlie doit 151€ à Alice
- Charlie doit 31€ à Bob

Bob jubile ! Mais est-ce que Charlie est satisfait par situation ? Il lui suffit de trouver une solution plus optimale. Je vous passe les détails, mais pour tester la solution optimale, il suffit de voir ce qu'il se passe dans notre équation à minimiser de départ \(`|X|` + `|Y|` + `|Z|`\) à partir de notre système d'équations, et les totaux obtenus sont supérieurs à notre solution, pour `Y` = 0 et `Z` = 0 \(qui valent respectivement 333 et 212\).

:::info
Revenons à la solution précédente :

- Bob doit 40€ à Alice
- Charlie doit 111€ à Alice
- Charlie doit 71€ à Bob

Oui, maintenant ça se voit que Charlie pouvait juste donner 31€ à Bob, et donner 151€ à Alice. C'est le fameux algorithme dont je parle en info rapidement. Mais hey, lis la suite et tu comprends pourquoi c'est... particulier
:::

## La généralisation

L'article a LARGEMENT explosé en taille... donc ce sera pour une partie 2...