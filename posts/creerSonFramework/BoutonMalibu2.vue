<template>
  <div v-html="html"></div>
</template>

<script setup lang="ts">
import { nextTick, onMounted, ref } from 'vue'

const boutonNirina = `<button class="button" clic="alert('maliba')">alerte à malibu</button>`

const html = ref('')

interface BoutonHtml {
  fonctionQuandOnClique: string // la fonction retourne unknown car je me fiche ici de savoir ce qu'elle fait
  texte: string // c'est techniquement pas ouf mais on en reparle après...
}

async function nirinajs(htmlNirina: string) {
  // parser l'élément qui nous intéresse
  const parsedHtml = new DOMParser().parseFromString(htmlNirina, 'text/html')

  const boutonAlerteAMalibu: BoutonHtml = {
    texte: parsedHtml.textContent ?? '',
    fonctionQuandOnClique:
      parsedHtml.querySelector('button')?.attributes.getNamedItem('clic')
        ?.value ?? ''
  }

  // créer l'objet cible qu'on veut
  const el = document.createElement('html')
  const bouton = document.createElement('button')

  // créer un id technique dont on n'a rien à faire à part pour le relier avec notre script
  bouton.setAttribute('id', 'bouton-qui-bouge')
  el.appendChild(bouton)
  // ajouter un élément scripts qui va contenir notre code
  const scriptElement = document.createElement('script')
  scriptElement.textContent = boutonAlerteAMalibu.fonctionQuandOnClique
  // ajouter cet élément script
  el.appendChild(scriptElement)

  return el.outerHTML
}

onMounted(() => {
  nirinajs(boutonNirina)
})
</script>

<style lang="css">
.button {
  background-color: red;
  border-radius: 2rem;
  padding: 0.5rem;
  color: white;
}

.button:hover {
  background-color: brown;
}

.button:active {
  background-color: green;
}
</style>
