<script setup>
import { ref, computed } from 'vue'
import FormularioTarefa from './components/FormularioTarefa.vue'
import ListaTarefas from './components/ListaTarefas.vue'

const tarefas = ref([
  { id: 1, titulo: 'Ler Vue', concluida: false },
  { id: 2, titulo: 'Revisar Vue', concluida: true },
  { id: 3, titulo: 'Organizar mesa', concluida: false },
])

const busca = ref('')
const situacao = ref('todas')

let proximoId = 4

function adicionarTarefa(titulo) {
  tarefas.value.push({ id: proximoId++, titulo, concluida: false })
}

function alternarTarefa(id) {
  const tarefa = tarefas.value.find((t) => t.id === id)
  if (tarefa) tarefa.concluida = !tarefa.concluida
}

function removerTarefa(id) {
  tarefas.value = tarefas.value.filter((t) => t.id !== id)
}

function limparFiltros() {
  busca.value = ''
  situacao.value = 'todas'
}

const pendentesCount = computed(
  () => tarefas.value.filter((t) => !t.concluida).length,
)

const tarefasFiltradas = computed(() => {
  const termo = busca.value.trim().toLowerCase()

  return tarefas.value.filter((tarefa) => {
    const combinaBusca = tarefa.titulo.toLowerCase().includes(termo)
    const combinaSituacao =
      situacao.value === 'todas' ||
      (situacao.value === 'pendentes' && !tarefa.concluida) ||
      (situacao.value === 'concluidas' && tarefa.concluida)

    return combinaBusca && combinaSituacao
  })
})
</script>

<template>
  <main>
    <header class="cabecalho">
      <h1>Minhas tarefas</h1>
      <span class="contador">{{ pendentesCount }} pendente(s)</span>
    </header>

    <FormularioTarefa @adicionar="adicionarTarefa" />

    <div class="filtros">
      <input
        v-model="busca"
        type="search"
        placeholder="Buscar por título..."
        aria-label="Buscar tarefas por título"
      />

      <select v-model="situacao" aria-label="Filtrar por situação">
        <option value="todas">Todas</option>
        <option value="pendentes">Pendentes</option>
        <option value="concluidas">Concluídas</option>
      </select>

      <button type="button" class="limpar" @click="limparFiltros">
        Limpar filtros
      </button>
    </div>

    <ListaTarefas
      :tarefas="tarefasFiltradas"
      @alternar="alternarTarefa"
      @remover="removerTarefa"
    />
  </main>
</template>

<style scoped>
.cabecalho {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: 24px;
}

h1 {
  font-size: 26px;
  margin: 0;
}

.contador {
  font-size: 14px;
  font-weight: 600;
  color: var(--accent);
  background: var(--accent-bg);
  padding: 4px 10px;
  border-radius: 999px;
}

.filtros {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.filtros input {
  flex: 1;
}

.filtros input,
.filtros select {
  padding: 10px 12px;
  border: 1px solid var(--border);
  border-radius: 6px;
  font-size: 14px;
  background: var(--surface);
  color: var(--text);
}

.filtros input:focus,
.filtros select:focus {
  outline: 2px solid var(--accent);
  outline-offset: 1px;
}

.limpar {
  padding: 10px 14px;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: var(--surface);
  color: var(--text-muted);
  font-size: 14px;
  cursor: pointer;
  white-space: nowrap;
}

.limpar:hover {
  color: var(--text);
  border-color: var(--text-muted);
}
</style>
