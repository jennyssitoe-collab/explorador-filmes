<script setup>
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'


const API_KEY = 'ca34f54b' 
const URL_BASE = 'https://www.omdbapi.com/'

const filmes = ref([])
const textoPesquisa = ref('') 
const anoFiltro = ref('')
const aCarregar = ref(false)
const erro = ref('')


async function buscarFilmes(termoInicial = null) {
  // Se recebermos um termo (ex: 'barbie'), usamos ele. Senão, usamos o que está na caixa.
  const termoParaBuscar = termoInicial || textoPesquisa.value
  
  if (termoParaBuscar === '') return
  
  aCarregar.value = true
  erro.value = ''

  try {
    let url = `${URL_BASE}?apikey=${API_KEY}&s=${termoParaBuscar}`
    if (anoFiltro.value !== '') {
      url += `&y=${anoFiltro.value}`
    }

    const resposta = await fetch(url)
    const dados = await resposta.json()

    if (dados.Response === 'True') {
      filmes.value = dados.Search 
    } else {
      filmes.value = []
      erro.value = dados.Error 
    }
  } catch (e) {
    erro.value = 'Erro ao ligar à internet.'
  } finally {
    aCarregar.value = false
  }
}


onMounted(() => {
  buscarFilmes('barbie')
})
</script>

<template>
  <div>
    <!-- Barra de Pesquisa -->
    <div style="margin-bottom: 30px; display: flex; gap: 10px; flex-wrap: wrap; justify-content: center;">
      
      <input 
        v-model="textoPesquisa" 
        @keyup.enter="buscarFilmes()"
        placeholder="pesquise o titulo do filme" 
        style="padding: 10px; width: 250px; font-size: 16px; border: 1px solid #ccc; border-radius: 4px;"
      >
      
      <input 
        v-model="anoFiltro" 
        type="number"
        @keyup.enter="buscarFilmes()"
        placeholder="pesquise o ano do filme" 
        style="padding: 10px; width: 220px; font-size: 16px; border: 1px solid #ccc; border-radius: 4px;"
      >

      
      <button @click="buscarFilmes()" style="padding: 10px 20px; font-size: 16px; cursor: pointer; background: #d63384; color: white; border: none; border-radius: 4px; font-weight: bold;">
        Procurar
      </button>

    </div>

    <p v-if="aCarregar">A carregar filmes...</p>
    <p v-else-if="erro" style="color: #d63384; font-weight: bold; text-align: center;">{{ erro }}</p>

    <!-- Lista de Filmes -->
    <div v-else style="display: flex; flex-wrap: wrap; gap: 20px; justify-content: center;">
      <div 
        v-for="filme in filmes" 
        :key="filme.imdbID" 
        style="border: 1px solid #f8bbd0; padding: 15px; width: 220px; background: white; border-radius: 8px; box-shadow: 0 4px 8px rgba(214, 51, 132, 0.1);"
      >
        <img 
          :src="filme.Poster !== 'N/A' ? filme.Poster : 'https://via.placeholder.com/200x300?text=Sem+Imagem'" 
          alt="Capa" 
          style="width: 100%; height: 320px; object-fit: cover; border-radius: 4px;"
        >
        <h3 style="font-size: 16px; margin: 10px 0; height: 40px; overflow: hidden; color: #333;">{{ filme.Title }}</h3>
        <p style="margin: 0; color: #666;">Ano: {{ filme.Year }}</p>
        
        <RouterLink 
          :to="'/filme/' + filme.imdbID" 
          style="display: block; margin-top: 15px; text-align: center; background: #d63384; color: white; padding: 8px; text-decoration: none; border-radius: 4px; font-size: 14px; font-weight: bold;"
        >
          Ver Detalhes
        </RouterLink>
      </div>
    </div>
  </div>
</template>