<script setup>
import ItemTarefa from './ItemTarefa.vue'

defineProps({
  tarefas: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['alternar', 'remover'])
</script>

<template>
  <p v-if="tarefas.length === 0" class="vazio">Nenhuma tarefa encontrada</p>

  <ul v-else class="lista">
    <ItemTarefa
      v-for="tarefa in tarefas"
      :key="tarefa.id"
      :tarefa="tarefa"
      @alternar="emit('alternar', $event)"
      @remover="emit('remover', $event)"
    />
  </ul>
</template>

<style scoped>
.lista {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.vazio {
  text-align: center;
  color: var(--text-muted);
  padding: 32px 0;
}
</style>
