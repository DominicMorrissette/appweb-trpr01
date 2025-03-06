<script setup lang="ts">
import { ref, defineProps, defineEmits, computed, watchEffect } from "vue"
import type { Game } from "../itemsType"
import ModalComponent from "./Modal.vue"

const props = defineProps<{ game: Game }>()
const emit = defineEmits(["removeGame", "saveUpdate", "duplicate"])

const isEditing = ref(false)
const isShowingDetails = ref(false)
const editedGame = ref({ ...props.game })
const showModal = ref(false)
const modalTitle = ref("")
const modalMessage = ref("")
const actionType = ref<"delete" | "edit" | "duplicate" | "BO" | null>(null)
const isLowStock = ref(false)

const errors = ref({
  title: "",
  description: "",
  price: "",
  rating: "",
  stock: "",
})

const deleteModal = () => {
  actionType.value = "delete"
  modalTitle.value = "Confirmation de suppression"
  modalMessage.value = "Êtes-vous sûr de vouloir supprimer ce jeu ?"
  showModal.value = true
}

const alertModal = () => {
  actionType.value = "BO"
  modalTitle.value = "Stock Faible"
  modalMessage.value =
    "Attention, le stock de " + props.game.title + " est faible (" + props.game.stock + " restant)."
  showModal.value = true
}

const confirmModal = (type: "duplicate" | "edit") => {
  actionType.value = type
  modalTitle.value = type === "duplicate" ? "Informations dupliqué" : "Modification enregistrée"
  modalMessage.value =
    type === "duplicate"
      ? "Informations dupliquées dans le form d'ajout."
      : "Les modifications ont été enregistrées avec succès."
  showModal.value = true
}

watchEffect(() => {
  if (isLowStock.value) {
    alertModal()
  }
})

const handleConfirm = () => {
  if (actionType.value === "delete") {
    emit("removeGame", props.game.id)
  }
  showModal.value = false
}

const validateFields = () => {
  errors.value = { title: "", description: "", price: "", rating: "", stock: "" }
  let isValid = true

  if (!editedGame.value.title.trim()) {
    errors.value.title = "Le titre est obligatoire."
    isValid = false
  }

  if (!editedGame.value.description.trim()) {
    errors.value.description = "La description est obligatoire."
    isValid = false
  }

  const priceNumber = parseFloat(editedGame.value.price as unknown as string)
  if (isNaN(priceNumber) || priceNumber <= 0) {
    errors.value.price = "Veuillez entrer un prix valide."
    isValid = false
  }

  const ratingNumber = parseFloat(editedGame.value.rating as unknown as string)
  if (isNaN(ratingNumber) || ratingNumber < 0 || ratingNumber > 5) {
    errors.value.rating = "Veuillez entrer une note entre 0 et 5."
    isValid = false
  }

  const stockNumber = parseInt(editedGame.value.stock as unknown as string)
  if (isNaN(stockNumber)) {
    errors.value.stock = "Le stock doit être renseigné."
    isValid = false
  } else if (stockNumber < 0) {
    errors.value.stock = "Le stock ne peut pas être négatif."
    isValid = false
  }

  return isValid
}

const saveEdit = () => {
  if (!validateFields()) return
  emit("saveUpdate", { ...editedGame.value })
  isEditing.value = false
  confirmModal("edit")
}

const startEdit = () => {
  editedGame.value = { ...props.game }
  isEditing.value = true
}

const cancelEdit = () => {
  errors.value.title = ""
  errors.value.description = ""
  errors.value.price = ""
  errors.value.rating = ""
  errors.value.stock = ""
  isEditing.value = false
}

const duplicate = () => {
  confirmModal("duplicate")
  emit("duplicate", { ...props.game })
}

const stockColor = computed(() => {
  if (props.game.stock > 100) {
    isLowStock.value = false
    return "bg-success text-white"
  } else if (props.game.stock > 50) {
    isLowStock.value = false
    return "bg-warning text-dark"
  } else {
    isLowStock.value = true
    return "bg-danger text-white"
  }
})

const formatPrice = (price: number) => {
  return new Intl.NumberFormat("fr-CA", { style: "currency", currency: "CAD" }).format(price)
}

const toggleDetails = () => {
  isShowingDetails.value = !isShowingDetails.value
}
</script>

<template>
  <div class="card p-3 mb-2">
    <ModalComponent
      :title="modalTitle"
      :message="modalMessage"
      :show="showModal"
      :type="actionType"
      @close="showModal = false"
      @confirm="handleConfirm"
    />

    <div class="d-flex justify-content-between align-items-center">
      <div class="flex-grow-1">
        <h5 v-if="!isEditing">{{ game.title }}</h5>
        <div v-else>
          <label for="rating">Titre</label>
          <input v-model="editedGame.title" class="form-control form-control-sm" />
          <span class="error" v-if="errors.title">{{ errors.title }}</span>
        </div>
      </div>
    </div>
    <div v-if="isShowingDetails && !isEditing">
      <p class="text-muted mb-1">
        {{ game.description }}
      </p>
    </div>
    <div v-if="isEditing">
      <label for="rating">Description</label>
      <textarea v-model="editedGame.description" class="form-control form-control-sm"></textarea>
      <span class="error" v-if="errors.description">{{ errors.description }}</span>
    </div>

    <div class="d-flex justify-content-between align-items-center">
      <span v-if="!isEditing" class="fw-bold text-success">{{ formatPrice(game.price) }}</span>
      <span v-if="!isEditing" class="text-warning">
        <!--Logo fait par ChatGPT-->
        <span v-for="n in Math.round(game.rating)" :key="n">⭐</span>
      </span>
      <span v-if="!isEditing" :class="[stockColor, 'p-1', 'rounded']">Stock: {{ game.stock }}</span>

      <div v-if="isEditing">
        <label for="price">Prix</label>
        <input
          id="price"
          v-model.number="editedGame.price"
          type="number"
          class="form-control form-control-sm"
          placeholder="Prix"
        />
        <span class="error" v-if="errors.price">{{ errors.price }}</span>
      </div>

      <div v-if="isEditing">
        <label for="rating">Note</label>
        <input
          id="rating"
          v-model.number="editedGame.rating"
          type="number"
          step="0.1"
          class="form-control form-control-sm"
          placeholder="Note"
        />
        <span class="error" v-if="errors.rating">{{ errors.rating }}</span>
      </div>

      <div v-if="isEditing">
        <label for="stock">Stock</label>
        <input
          id="stock"
          v-model.number="editedGame.stock"
          type="number"
          class="form-control form-control-sm"
          placeholder="Stock"
        />
        <span class="error" v-if="errors.stock">{{ errors.stock }}</span>
      </div>
    </div>

    <div class="d-flex gap-2 mt-2 justify-content-evenly">
      <!--Logo fait par ChatGPT-->
      <button v-if="!isEditing" class="btn btn-sm btn-outline-primary" @click="startEdit">
        ✏️ Modifier
      </button>
      <!--Logo fait par ChatGPT-->
      <button v-if="!isEditing" class="btn btn-sm btn-outline-info" @click="toggleDetails">
        {{ isShowingDetails ? "🔼 Masquer détails" : "📜 Détails" }}
      </button>
      <!--Logo fait par ChatGPT-->
      <button v-if="!isEditing" class="btn btn-sm btn-outline-success" @click="duplicate">
        📑 Dupliquer
      </button>
      <div v-else class="d-flex gap-3 justify-content-evenly">
        <!--Logo fait par ChatGPT-->
        <button
          class="btn btn-sm btn-success"
          @click="
            () => {
              saveEdit()
            }
          "
        >
          ✅ Sauvegarder
        </button>
        <!--Logo fait par ChatGPT-->
        <button class="btn btn-sm btn-secondary" @click="cancelEdit">❌ Annuler</button>
      </div>
      <!--Logo fait par ChatGPT-->
      <button class="btn btn-sm btn-outline-danger" @click="deleteModal()">🗑️ Retirer</button>
    </div>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
}

.error {
  color: red;
  font-size: 0.9em;
  display: block;
  margin-top: 2px;
}
</style>
