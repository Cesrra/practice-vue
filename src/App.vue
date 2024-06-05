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

const loadData = async () => {
  const respose = await fetch('https://api.imgflip.com/get_memes')
  const { data } = await respose.json()

  console.log(data.memes)
  //Forma de asignar valores a variables reactivas | memes = data.memes --> daría error
  memes.value = data.memes
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
    <input class="base-input" type="text" placeholder="Buscar Meme" />
    <section class="container">
      <!-- v-for es una de las Directivas principales -->
      <!-- v-bind:property_name = :property_name | Bind quiere decir que el valor de esa propiedad es dinámica -->
      <div v-for="meme in memes" :key="meme.id" class="card" >
        <!-- <HeartIcon class="icon" /> -->
        <p>{{ meme.name }}</p>
        <img :src="meme.url" :alt="meme.name">
      </div>
    </section>
  </header>

  <!-- <main>
    <TheWelcome />
  </main> -->
</template>

<!-- Si queiro que los estilos sólo se apliquen al componente usamos scoped como propiedad del style -->
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

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
