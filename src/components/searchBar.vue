<script setup lang="ts">
import { defineProps, defineEmits } from "vue"

const props = defineProps<{ modelValue: string }>()
const emit = defineEmits(["update:modelValue"])
</script>

<template>
  <div class="input-group">
    <input
      type="text"
      class="form-control"
      placeholder="Rechercher un jeu..."
      :value="props.modelValue"
      @input="emit('update:modelValue', ($event.target as HTMLInputElement).value)"
    />
    <!--Logo fait par ChatGPT-->
    <span class="input-group-text"> 🔍 </span>
  </div>
</template>

<style scoped>
.input-group-text {
  background: #f8f9fa;
  border-left: 0;
  cursor: pointer;
}
</style>
