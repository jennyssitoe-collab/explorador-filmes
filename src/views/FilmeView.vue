<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const API_KEY = 'ca34f54b' // <-- COLE A SUA CHAVE AQUI!
const filme = ref(null)
const aCarregar = ref(true)

onMounted(async () => {
  try {
    const resposta = await fetch(`https://www.omdbapi.com/?apikey=${API_KEY}&i=${route.params.id}`)
    filme.value = await resposta.json()
  } catch (e) {
    console.error(e)
  } finally {
    aCarregar.value = false
  }
})
</script>

<template>
  <div>
    <p v-if="aCarregar">A carregar detalhes...</p>
    <div v-else-if="filme" style="display: flex; gap: 30px; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);">
      
      <img :src="filme.Poster !== 'N/A' ? filme.Poster : 'https://via.placeholder.com/300x450?text=Sem+Imagem'" style="width: 300px; border-radius: 8px;">
      
      <div>
        <h2 style="margin-top: 0;">{{ filme.Title }} ({{ filme.Year }})</h2>
        <p><strong>Género:</strong> {{ filme.Genre }}</p>
        <p><strong>Duração:</strong> {{ filme.Runtime }}</p>
        <p><strong>Realizador:</strong> {{ filme.Director }}</p>
        <p><strong>Sinopse:</strong> {{ filme.Plot }}</p>
        <p><strong>Nota IMDb:</strong> ⭐ {{ filme.imdbRating }}</p>
        
        <RouterLink to="/" style="display: inline-block; margin-top: 20px; background: #1a1a2e; color: white; padding: 10px 20px; text-decoration: none; border-radius: 4px;">
          ← Voltar para a lista
        </RouterLink>
      </div>
    </div>
  </div>
</template>