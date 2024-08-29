<script setup lang="ts">
import { reactive } from 'vue';

  const estado = reactive({
    tarefaTemp: '',
    filtro: 'todas',
    tarefas: [
      {
        titulo: 'Estudar ES',
        finalizada: false
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true
      }
    ]
  });

  const retornaTarefasPendentes = () =>{
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  }

  const retornaTarefasFinalizadas = () =>{
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  }

  const retornaTarefasFiltradas = () =>{
    const { filtro } = estado;

    switch (filtro){
      case 'pendentes' :
        return retornaTarefasPendentes();
      case 'finalizadas' :
        return retornaTarefasFinalizadas();
      default : 
        return estado.tarefas;
    }
  }

  const cadastraTarefa = (e: any) =>{
    e.preventDefault();
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
  // 
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ retornaTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <!-- efetua o prevent com '.prevent' -->
    <form action="" @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="(evento: any) => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button class="btn btn-primary" type="submit">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="(evento: any) => estado.filtro = evento.target.value" name="" id="" class="form-control">
            <option value="todas">Todas as tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in retornaTarefasFiltradas()">
        <input @change="(evento: any) => tarefa.finalizada = evento.target.checked" type="checkbox" :checked="tarefa.finalizada" :id="tarefa.titulo">
        <label :class="{'pronto': tarefa.finalizada}" :for="tarefa.titulo" class="form-label ms-3">{{ tarefa.titulo }}</label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .pronto{
    text-decoration: line-through;
  }
</style>
