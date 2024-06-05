<!-- 
- Se concoe como SFC(single file compoenent) 
- Composition API en Vue 3 [script, template, style]
- El único requerido es el template
- Hay dos formas de escribir el script en Composition API
-->

<!-- Forma #1 -->
<script setup>
import { onMounted, ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
  
// ref --> genera una variable reactiva
const memes = ref([])
const favorites = ref([])
let allMemes = []

const loadData = async () => {
  const respose = await fetch('https://api.imgflip.com/get_memes')
  const { data } = await respose.json()

  console.log(data.memes)
  //Forma de asignar valores a variables reactivas | memes = data.memes --> daría error
  memes.value = data.memes
  allMemes = data.memes
}

// Recibe el evento como parametro de la función
const searchMeme = (event) => {
  // Este event tiene la información del elemento que se está escuchando
  console.log(event.target.value)
  const { value } = event.target
  // En el template no necesitas acceder al valuer, pero en la logica si
  memes.value = allMemes.filter(meme => meme.name.toLowerCase().includes(value.toLowerCase()))
}

const handleFav = (memeId) => {
  const index = favorites.value.indexOf(memeId)

  if(index > -1) {
    favorites.value.splice(index, 1)
  }else {
    favorites.value.push(memeId)
  }

  console.log(favorites.value)
}

// Si quisieramos ejecutar codigo cuando se crea el componente
loadData()

// Si quisieramos ejecutar codigo cuando el componente esté montado
onMounted(() => {
  console.log('Montado')
})
</script>

<!--  Forma #2
<script>
/*
La diferencia principal es que en la forma #2 necesita retornar 
todo lo que se valla a utilizar en el template
*/
  export default {
    setup() {
      const memes = ref([])
      const loadData = async () => {
        const respose = await fetch('https://api.imgflip.com/get_memes')
        const { data } = await respose.json()

        console.log(data.memes)
        memes.value = data.memes
      }

      loadData()

      return {
        memes,
      }
    },
  }
</script>
-->

<!-- Option API en Vue 2 
<script>
  export default {
    name: 'App',
    mounted() {},
    methods: {
      async loadData() {
        const respose = await fetch('https://api.imgflip.com/get_memes')
        const { data } = await respose.json()
        console.log(data.memes)
      }
    },
  }
</script>
-->

<template>
  <header>
    <h1>Meme Search</h1>

    <!-- v-on:property_name = @property_name | On quiere decir que está escuchando ese evento -->
    <div class="search">      
      <input class="base-input" type="text" placeholder="Buscar Meme" @input="searchMeme" />
    </div>

    <section class="container">
      <!-- v-for es una de las Directivas principales -->
      <!-- v-bind:property_name = :property_name | Bind quiere decir que el valor de esa propiedad es dinámica -->
      <div v-for="meme in memes" :key="meme.id" class="card" >
        <p>{{ meme.name }}</p>
        <!-- Aplicamos clases condicionales, eventos On de escucha -->
        <img :src="meme.url" :alt="meme.name">
        <div class="icon" :class="{ selected: favorites.includes(meme.id) }" @click="handleFav(meme.id)" ></div>
      </div>
    </section>
  </header>

  <!-- <main>
    <TheWelcome />
  </main> -->
</template>

<!-- Si queiro que los estilos sólo se apliquen al componente usamos <style scoped> -->
<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

header h1 {
  text-align: center;
  padding: 1%;
}

.search {
  display: flex;
  padding: 2.5%;
  position: sticky;
  top: 0;
  z-index: 100;
  background-color: white;
  box-shadow: 0 2px 8px 0;
  margin-bottom: 13px;
}

.base-input {
  width: 100%;
}

.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 20px;
}

.icon {
  width: 25px;
  height: 25px;
  margin: 5px;
  background-color: white;
  left: 75%;
  border: solid 2px;
  border-radius: 1rem;
}

.card {
  max-width: 180px;
  height: auto;
  padding: 2%;
  border: solid 2px;
  border-radius: .5rem;
}

.card img {
  width: 100%;
  height: auto;
  object-fit: cover;
}

.card p {
  font-weight: bold;
  text-align: center;
  margin: 5px;
}

.selected {
  background-color: blue;
  border-color: #ff3b00a3;
}

</style>
