<script setup>
defineProps({
  tarefa: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['alternar', 'remover'])
</script>

<template>
  <li class="item" :class="{ concluida: tarefa.concluida }">
    <label class="titulo">
      <input
        type="checkbox"
        :checked="tarefa.concluida"
        @change="emit('alternar', tarefa.id)"
      />
      <span>{{ tarefa.titulo }}</span>
    </label>

    <button type="button" class="remover" @click="emit('remover', tarefa.id)" aria-label="Remover tarefa">
      Remover
    </button>
  </li>
</template>

<style scoped>
.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px 14px;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: var(--surface);
}

.titulo {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.item.concluida .titulo span {
  color: var(--text-muted);
  text-decoration: line-through;
}

input[type='checkbox'] {
  accent-color: var(--done);
  width: 16px;
  height: 16px;
}

.remover {
  border: none;
  background: none;
  color: var(--danger);
  font-size: 13px;
  cursor: pointer;
  padding: 4px 6px;
}

.remover:hover {
  text-decoration: underline;
}
</style>
