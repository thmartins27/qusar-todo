<template>
  <q-page class="bg-grey-3 column">

    <div class="row q-pa-sm bg-primary">
      <q-input @keyup.enter="addTask" v-model="newTask" class="col" square bg-color="white" filled
        placeholder="Add task" dense>

        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>

      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item v-for="(task, index) in tasks" :key="index" @click="changeDone(index)" v-ripple clickable
        :class="{ 'done bg-blue-1': task.done }"> <!-- função para  mudar o done-->
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events" />
        </q-item-section>


        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="red" icon="delete" />
        </q-item-section>
      </q-item>

    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">

      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">
        No Tasks
      </div>

    </div>


  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({

  data: () => ({
    newTask: '',
    tasks: []
  }),

  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        localStorage.setItem('items', JSON.stringify(this.tasks))
        this.$q.notify({
          message: 'task delete',
          icon: 'announcement',
          group: false,
        })
      })

    },
    addTask() {
      if (this.newTask == '') {
        this.$q.notify({
          message: 'blank task',
          icon: 'announcement',
          group: false
        })
      } else {
        this.tasks.push({
          title: this.newTask,
          done: false
        })
        localStorage.setItem('items', JSON.stringify(this.tasks))
        this.newTask = ''
      }

    },
    changeDone(index) {
      this.tasks[index].done = !this.tasks[index].done
      localStorage.setItem('items', JSON.stringify(this.tasks))
    }
  },

  mounted () {
      const tarefas = localStorage.getItem('items')
      if(tarefas) {
        this.tasks = JSON.parse(tarefas)
      }

  }


})
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
