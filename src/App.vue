<template>
  <div>
    <h1 class="cover-heading">My To Do App</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTaskName" type="text" class="form-control">
      <input type="submit" value="Agregar Tarea" class="btn btn-lg btn-block btn-primary">
    </form>

    <tasks-list
      :title="'Pendientes'"
      :task-list="tasksPending"
      @completar="toggleTasks"/>

    <tasks-list
      :title="'Completados'"
      :task-list="tasksComplete"
      @completar="toggleTasks"/>

  </div>
</template>

<script>
import TasksList from './components/TasksList'

export default {
  components: {
    TasksList
  },
  data () {
    return {
      newTaskName: '',
      tasks: [
        {
          name: 'Tarea 1',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        }
      ]
    }
  },
  created(){
    let tmp = localStorage.getItem('tasks')
    if (tmp != null) {
      this.tasks = JSON.parse(tmp)
    } else {
      this.tasks = []
    }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    addTask () {
      if (!this.newTaskName) return
      this.tasks.push({ name: this.newTaskName, done: false })
      this.newTaskName = ''
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  },
  computed: {
    tasksPending () {
      return this.tasks.filter(task => !task.done)
    },
    tasksComplete () {
      return this.tasks.filter(task => task.done)
    }
  }
}
</script>