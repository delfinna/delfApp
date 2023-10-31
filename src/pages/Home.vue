<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input v-model="newTask" class="col" square filled
        bg-color="white"
        placeholder="Adicionar"
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks" :key="task.title"
        @click="task.done= !task.done"
        :class="{ 'done  bg-blue-1' : task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox class="no-pointer-events" v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
           flat round dense
           color="primary" icon="delete"
           @click.stop="deleteTask(index)"/>
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length"  class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center"> Sem Tarefas</div>
    </div>

  </q-page>
</template>

<script>
import { ref } from 'vue'

export default {
  data () {
    return {
      newTask: '',
      tasks: [
      ]
    }
  },
  methods: {
    deleteTask (index) {
      this.$q.dialog({
        title: 'Confirm',
        messsage: 'Tens certeza que quer deletar',
        cancel: true,
        parsistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Apagado.')
      })
    },
    addTask () {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  },
  setup () {
    return {
      slide: ref('style'),
      lorem: 'Lorem ipsum dolor, sit amet consectetur adipisicing elit. Itaque voluptatem totam, architecto cupiditate officia rerum, error dignissimos praesentium libero ab nemo.'
    }
  }
}
</script>

<style lang="scss">
.done{
  .q-item__label {
    text-decoration: line through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
