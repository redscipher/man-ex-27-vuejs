<script setup lang="ts">
  import { reactive } from 'vue';
  import Cabecalho from './componentes/Cabecalho.vue';
  import Formulario from './componentes/Formulario.vue';
  import ListaTarefas from './componentes/ListaTarefas.vue';

  let flgTarefas = false;

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

  const cadastraTarefa = (e: any): void =>{
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
    <Cabecalho :tarefas-pendentes="retornaTarefasPendentes().length"></Cabecalho>
    <Formulario :cadastra-tarefa="cadastraTarefa" :edita-tarefa-temp="(evento: any) => estado.tarefaTemp = evento.target.value" :tarefa-temp="estado.tarefaTemp" :trocar-filtro="(evento: any) => estado.filtro = evento.target.value"></Formulario>
    <ListaTarefas :tarefas-filtradas="retornaTarefasFiltradas()"></ListaTarefas>
  </div>
</template>
