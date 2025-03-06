<script setup lang="ts">
import { defineProps } from "vue"

const props = defineProps<{
  games: Array<{
    id: number
    title: string
    price: number
    description: string
    rating: number
    stock: number
  }>
}>()

//Aide avec ChatGPT pour comprendre et donner des indication sur la façon de faire le CSV (voir ReadMe pour le prompt et la réponse)
const exportCSV = () => {
  const csvHeader = "ID,Titre,Prix,Description,Note,Stock\n"
  const csvRows = props.games.map(
    (game) =>
      `${game.id},"${game.title}",${game.price},"${game.description}",${game.rating},${game.stock}`
  )

  const csvContent = csvHeader + csvRows.join("\n")
  const blob = new Blob([csvContent], { type: "text/csv" })
  const url = URL.createObjectURL(blob)

  const a = document.createElement("a")
  a.href = url
  a.download = "jeux.csv"
  a.click()
  URL.revokeObjectURL(url)
}
</script>

<template>
  <!--Logo fait avec ChatGPT-->
  <button class="btn btn-success" @click="exportCSV">📤 Exporter CSV</button>
</template>

<style scoped>
button {
  display: flex;
  align-items: center;
  gap: 5px;
}
</style>
