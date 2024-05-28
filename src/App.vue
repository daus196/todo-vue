<script setup>
import { reactive } from 'vue';
import Header from './components/Header.vue'
import Form from './components/Form.vue'
import Lista from './components/Lista.vue'

const estado = reactive({
  filtro: 'todas',
  tarefaTemp:'',
  tarefas:[
    {
      titulo:'Estudar ES',
      finalizada:false,
    },
    {
      titulo:'Estudar Sass',
      finalizada: false,
    },
    {
      titulo:'ir para a academia',
      finalizada:true,
    }
  ]
})

const getTarefasPendentes = () =>{
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}
const getTarefasFinalizadas = () =>{
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () =>{
  const {filtro} = estado;

  switch (filtro){
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () =>{
  const tarefaNova = {
  titulo: estado.tarefaTemp,
  finalizada: false,
}
estado.tarefas.push(tarefaNova)
estado.tarefaTemp=''
}
</script>

<template>
<div class="container">
<Header :tarefa-pendentes="getTarefasPendentes().length"></Header>
<Form :trocar-filtro="evento => estado.filtro = evento.target.value":tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"></Form>
<Lista :tarefas="getTarefasFiltradas()"></Lista>
</div>
</template>


