<script setup>
import { ref } from 'vue'

const emit = defineEmits(['adicionar'])

const titulo = ref('')

function enviar() {
  const valor = titulo.value.trim()
  if (!valor) return

  emit('adicionar', valor)
  titulo.value = ''
}
</script>

<template>
  <form class="formulario" @submit.prevent="enviar">
    <input
      v-model="titulo"
      type="text"
      placeholder="Nova tarefa..."
      aria-label="Título da nova tarefa"
    />
    <button type="submit">Adicionar</button>
  </form>
</template>

<style scoped>
.formulario {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 10px 12px;
  border: 1px solid var(--border);
  border-radius: 6px;
  font-size: 15px;
  background: var(--surface);
  color: var(--text);
}

input:focus {
  outline: 2px solid var(--accent);
  outline-offset: 1px;
}

button {
  padding: 10px 16px;
  border: none;
  border-radius: 6px;
  background: var(--accent);
  color: #fff;
  font-size: 15px;
  cursor: pointer;
}

button:hover {
  opacity: 0.9;
}
</style>
