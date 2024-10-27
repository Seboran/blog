---
title: Comment créer son framework Javascript en 2024
date: 2024-11-01
author: Nirina Rabeson
---
<script setup>
import BoutonQuiBougeALancienne from './BoutonQuiBougeALancienne.vue'
import BoutonMalibu2 from "./creerSonFramework/BoutonMalibu2.vue"
</script>

![photo de tuyaux en acier dans une usine](/crystal-kwok-xD5SWy7hMbw-unsplash.jpg)

[Svelte 5](https://svelte.dev/blog/svelte-5-is-alive), [React 19](https://react.dev/blog/2024/04/25/react-19), [Next 15](https://nextjs.org/blog/next-15), [htmx 2](https://htmx.org/) regardez tous ces nouveaux frameworks qui se mettent à jour et qui évoluent continuellement !

Et si on faisait partie du problème en créant notre problème framework Javascript ? Regardons pourquoi et surtout, **comment** !!


## C'est vachement dur de faire du web natif

De mon temps, c'était difficile de faire du web. Imaginez faire un bouton qui permet juste d'afficher une alerte à Malibu juste en cliquant dessus sans votre stack technique, comme ça : <BoutonQuiBougeALancienne />

Comment écrire un bouton pareil, aussi stylé, en html js natif ?

```html
<button id="bouton-qui-bouge">alerte à malibu</button>

<script>
document.querySelector('#bouton-qui-bouge').addEventListener('click', () => {
  alert('malibu')
})
</script>
```

**C'est très, vintage... 💅🏼** !!! Qu'est-ce qu'on pourrait faire pour améliorer ce code ? Que veut dire "améliorer" dans ce cas ? Regardons plusieurs améliorations possibles

---

::: info
Vous remarquerez que le style du bouton est dynamique. C'est du pur css qui permet de faire ça... donc nous ne parlerons ni de style ni d'animations ici car c'est déjà tout possible en pur css.
:::

## Améliorer la lisibilité

Il y a beaucoup de termes techniques dans juste ce bouton, sont-ils pertinents pour nos usages ? Dans notre cas, je veux un `button`, et quand je `clic` dessus, je veux voir une `alert` contenant le mot *malibu*. Je me fiche de nommer un `id`, de faire un `querySelector`, et qu'est-ce qu'un `addEventListener` ? *\(sic\)*. 

Avons-nous un moyen d'abstraire ces mots techniques pour ne garder que ceux qui sont fonctionnels ? Il y a deux méthodes, une très compliquée et une autre tout aussi compliquée... **Regardons d'abord la transpilation.**

### Transpiler ? pardon ?

Dans un monde idéal et francophone, je voudrais bien écrire la chose suivante :

```html
<button clic="alert('malibu')">alerte à malibu</button>
```

Ce code est génial car il ne contient presque que les termes qui nous concernent. Hélas, il ne marche pas du tout. Serait-il possible d'écrire ce code ainsi et le transformer en code ? C'est ce que la transpilation nous permet de faire.

Nous allons créer une fonction qu'on va nommer `nirinajs` qui prend le petit bout de html en langage simple, et qui va nous retourner un html compatible avec l'api du navigateur. Par exemple, elle sera capable de nous produire le rendu suivant :

```ts
const boutonNirina = `<button clic="alert('malibu')">alerte à malibu</button>`
const boutonHtml = nirinajs(boutonNirina)
console.log(boutonHtml)
```

Et en sortie nous voulons le code suivant qui ressemblera à quelque chose

```html
<button id="un-id-dont-je-me-fiche">alerte à malibu</button>

<script>
document.querySelector('#un-id-dont-je-me-fiche').addEventListener('click', () => {
  alert('malibu')
})
```

Comment faire marcher une pareille fonction ? Modélisons d'abord notre syntaxe idéale de bouton en faisant la liste des propriétés qu'il doit avoir :

- c'est un bouton
- il reçoit une fonction qui se déclenche quand on clique dessus
- il contient du texte

On peut en déduire une interface qui ressemblerait à ça :

```ts
interface BoutonHtml {
  fonctionQuandOnClique: () => unknown; // la fonction retourne unknown car je me fiche ici de savoir ce qu'elle fait
  texte: string; // c'est techniquement pas ouf mais on en reparle après...
}
```

Pour écrire notre bouton idéal, nous pouvons écrire objet qui implémente l'interface ainsi :

```ts
const monBoutonIdeal: BoutonHtml = {
  fonctionQuandOnClique: () => alert('malibu');
  texte: "alerte à malibu";
}
```



<BoutonMalibu2 />

Si vous avez envie d'aller plus loin en transpilation, j'ai un talk entier sur le sujet : <https://www.youtube.com/watch?v=Q3D8sGoS9PA>
