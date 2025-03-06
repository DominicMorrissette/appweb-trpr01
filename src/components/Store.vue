<script setup lang="ts">
import { computed, ref } from "vue"
import GameItem from "./GameItem.vue"
import SearchBar from "./searchBar.vue"
import { type Game } from "../itemsType"
import ExportCSV from "./ExportCSV.vue"

const games = ref<Game[]>([
  {
    id: 1,
    title: "Cyberpunk 2077",
    price: 59.99,
    description:
      "Plongez dans l'univers de Cyberpunk grâce au jeu original Cyberpunk 2077, avec son extension Phantom Liberty, qui mêle espionnage et frissons, ou en regardant la série animée Cyberpunk: Edgerunners. Il y a tout un tas d'histoires à découvrir au cœur de la dangereuse mégalopole de Night City.",
    rating: 4.6,
    stock: 200,
  },
  {
    id: 2,
    title: "Zelda: Tears of the Kingdom",
    price: 69.99,
    description:
      "Explorez un vaste monde ouvert rempli d'énigmes, de donjons et d'aventures dans la suite de Breath of the Wild. Affrontez Ganondorf et découvrez les secrets du royaume d'Hyrule.",
    rating: 3.3,
    stock: 150,
  },
  {
    id: 3,
    title: "Elden Ring",
    price: 79.99,
    description:
      "Plongez dans le vaste monde de l'Entre-Terre conçu par Hidetaka Miyazaki et George R.R. Martin. Un RPG en monde ouvert avec un gameplay exigeant et un lore fascinant.",
    rating: 4.8,
    stock: 34,
  },
  {
    id: 4,
    title: "Dark Souls 3",
    price: 39.99,
    description:
      "L'ultime volet de la saga Dark Souls vous propose un défi de taille avec ses combats intenses et son univers sombre. Défiez les seigneurs des Cendres et découvrez la vérité sur Lothric.",
    rating: 4.7,
    stock: 100,
  },
  {
    id: 5,
    title: "Hollow Knight",
    price: 19.99,
    description:
      "Un Metroidvania magnifique et exigeant où vous explorez le royaume en ruine d'Hallownest, affrontez des ennemis redoutables et découvrez une histoire captivante.",
    rating: 4.3,
    stock: 250,
  },
  {
    id: 6,
    title: "Celeste",
    price: 14.99,
    description:
      "Un jeu de plateforme exigeant où vous incarnez Madeline, une jeune femme cherchant à gravir la montagne Celeste tout en affrontant ses propres démons intérieurs.",
    rating: 4.8,
    stock: 180,
  },
])

const newGameTitle = ref("")
const newPrice = ref("")
const newDescription = ref("")
const newRating = ref("")
const newStock = ref("")
const searchQuery = ref("")
const gameId = ref(games.value.length > 0 ? Math.max(...games.value.map((game) => game.id)) + 1 : 1)
const errors = ref({
  title: "",
  price: "",
  description: "",
  rating: "",
  stock: "",
})

const validateFields = () => {
  errors.value = { title: "", price: "", description: "", rating: "", stock: "" }
  let isValid = true

  if (!newGameTitle.value.trim()) {
    errors.value.title = "Le titre est obligatoire."
    isValid = false
  }

  const priceNumber = parseFloat(newPrice.value)
  if (isNaN(priceNumber) || priceNumber <= 0) {
    errors.value.price = "Veuillez entrer un prix valide."
    isValid = false
  }

  if (!newDescription.value.trim()) {
    errors.value.description = "La description est obligatoire."
    isValid = false
  }

  const ratingNumber = parseFloat(newRating.value)
  if (isNaN(ratingNumber) || ratingNumber < 0 || ratingNumber > 5) {
    errors.value.rating = "Veuillez entrer une note entre 0 et 5."
    isValid = false
  }

  const stockNumber = parseInt(newStock.value)
  if (isNaN(stockNumber)) {
    errors.value.stock = "Le stock doit être renseigné."
    isValid = false
  } else if (stockNumber < 0) {
    errors.value.stock = "Le stock ne peut pas être négatif."
    isValid = false
  }

  return isValid
}

const addGame = () => {
  if (!validateFields()) return

  games.value.push({
    id: gameId.value++,
    title: newGameTitle.value.trim(),
    price: parseFloat(newPrice.value),
    description: newDescription.value.trim(),
    rating: parseFloat(newRating.value),
    stock: parseInt(newStock.value),
  })

  newGameTitle.value = ""
  newPrice.value = ""
  newDescription.value = ""
  newRating.value = ""
  newStock.value = ""
}

const removeGame = (id: number) => {
  games.value = games.value.filter((game) => game.id !== id)
}

const saveUpdate = (updatedGame: Game) => {
  const index = games.value.findIndex((game) => game.id === updatedGame.id)
  if (index !== -1) {
    games.value[index] = { ...updatedGame }
  }
}

const duplicateGame = (game: Game) => {
  newGameTitle.value = game.title + " (copie)"
  newPrice.value = game.price.toString()
  newDescription.value = game.description
  newRating.value = game.rating.toString()
  newStock.value = game.stock.toString()
}

const filteredGames = computed(() => {
  return games.value.filter((game) =>
    game.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-12">
        <img src="../assets/sitename.png" alt="Science des jeux" class="img-fluid" />
        <div class="card p-3">
          <h5 class="text-center mb-3">Ajouter un jeu</h5>

          <div class="d-flex flex-column gap-2">
            <div>
              <label for="rating">Titre</label>
              <input v-model="newGameTitle" type="text" class="form-control" placeholder="Titre" />
              <span class="text-danger" v-if="errors.title">{{ errors.title }}</span>
            </div>
            <div>
              <label for="rating">Description</label>
              <input
                v-model="newDescription"
                type="text"
                class="form-control"
                placeholder="Description"
              />
              <span class="text-danger" v-if="errors.description">{{ errors.description }}</span>
            </div>
            <div class="d-flex flex-wrap gap-2">
              <div class="flex-grow-1">
                <label for="rating">Prix</label>
                <input v-model="newPrice" type="number" class="form-control" placeholder="Prix" />
                <span class="text-danger" v-if="errors.price">{{ errors.price }}</span>
              </div>
              <div class="flex-grow-1">
                <label for="rating">Note</label>
                <input
                  v-model="newRating"
                  type="number"
                  step="0.1"
                  class="form-control"
                  placeholder="Note"
                />
                <span class="text-danger" v-if="errors.rating">{{ errors.rating }}</span>
              </div>
              <div class="flex-grow-1">
                <label for="rating">Stock</label>
                <input v-model="newStock" type="number" class="form-control" placeholder="Stock" />
                <span class="text-danger" v-if="errors.stock">{{ errors.stock }}</span>
              </div>
            </div>
          </div>

          <div class="d-flex flex-column gap-2 mb-3">
            <button class="btn btn-primary w-100" @click="addGame">➕ Ajouter</button>
            <div class="d-flex justify-content-between">
              <SearchBar v-model="searchQuery" class="flex-grow-1" />
              <ExportCSV :games="games" class="ms-2" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="filteredGames.length > 0" class="row mt-3 row-cols-1 row-cols-md-3 g-3">
      <div v-for="game in filteredGames" :key="game.id" class="col">
        <GameItem
          :game="game"
          @removeGame="removeGame"
          @saveUpdate="saveUpdate"
          @duplicate="duplicateGame"
        />
      </div>
    </div>
    <p v-else class="text-center mt-3">Aucun jeu trouvé.</p>
  </div>
</template>

<style scoped>
.text-danger {
  font-size: 0.875rem;
}

.card {
  min-width: 250px;
}
</style>
