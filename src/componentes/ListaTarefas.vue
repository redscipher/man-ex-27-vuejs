<script setup lang="ts">
    const props = defineProps(['tarefasFiltradas']);

    function validaTarefas(): boolean{
        return props.tarefasFiltradas.length > 0 ? true: false;
    }
</script>

<template>
    <div class="msg" v-if="!validaTarefas()">
        <span>Não possui tarefas neste filtro.</span>
    </div>
    <ul v-else class="list-group mt-4">
        <li class="list-group-item" v-for="tarefa in props.tarefasFiltradas">
            <input @change="(evento: any) => tarefa.finalizada = evento.target.checked" type="checkbox" :checked="tarefa.finalizada" :id="tarefa.titulo">
            <label :class="{'pronto': tarefa.finalizada}" :for="tarefa.titulo" class="form-label ms-3">{{ tarefa.titulo }}</label>
        </li>
    </ul>
</template>

<style scoped>
  .pronto{
    text-decoration: line-through;
  }

  .msg{
    margin-top: 16px;
    padding-top: 16px;
  }
</style>