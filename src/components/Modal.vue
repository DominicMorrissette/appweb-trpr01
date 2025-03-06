<script setup lang="ts">
import { defineProps, defineEmits, watchEffect } from "vue"

const props = defineProps<{
  title: string
  message: string
  show: boolean
  type: "delete" | "edit" | "duplicate" | "BO" | null
}>()

const emit = defineEmits(["close", "confirm"])

watchEffect(() => {
  if (props.show && props.type === "BO") {
    setTimeout(() => {
      emit("close")
    }, 3000)
  }
})
</script>

<template>
  <!--Aide de la documentation de boostrap sur les modal (merci Jimmy pour l'idée)-->
  <div v-if="show" class="modal fade show d-block" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ title }}</h5>
          <button type="button" class="btn-close" @click="emit('close')"></button>
        </div>
        <div class="modal-body">
          <p>{{ message }}</p>
        </div>
        <div class="modal-footer">
          <button v-if="type === 'delete'" class="btn btn-secondary" @click="emit('close')">
            Annuler
          </button>
          <button v-if="type !== 'BO'" class="btn btn-primary" @click="emit('confirm')">
            Confirmer
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal {
  background: rgba(0, 0, 0, 0.5);
}
</style>
