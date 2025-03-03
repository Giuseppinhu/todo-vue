<script setup>
    import { reactive } from 'vue';
    import Header from './components/Header.vue';
    import Form from './components/Form.vue';
    import ToDoList from './components/ToDoList.vue';


  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: [],
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para a Academia',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
        break;
      case 'finalizadas':
        return getTarefasFinalizadas();
        break;
      default:
        return estado.tarefas;
        break;
    }
  }

  const cadastrarTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Header :tarefas-pendentes="getTarefasPendentes().length"/>
    <Form :trocar-filtro="e => estado.filtro = e.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="e => estado.tarefaTemp = e.target.value" :cadastra-tarefa="cadastrarTarefa"/>
    <ToDoList :tarefas="getTarefasFiltradas()"/>
  </div>
</template>

