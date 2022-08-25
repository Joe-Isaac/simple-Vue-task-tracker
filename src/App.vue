<template>
<div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
    <div v-show="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
 </div>
</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks.vue';
import AddTask from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    Tasks,
    AddTask,
},
data() {
  return {
    tasks: [],
    showAddTask: false,
  }
},

methods: {
  toggleAddTask(){
    this.showAddTask = !this.showAddTask;
  },
  async addTask(task){
    console.log(task)
    await fetch('http://localhost:5000/tasks', {
      method: 'POST',
      headers: {
        'Content-type' : 'application/json'
      },
      body: JSON.stringify(task)
    })
    .then(
      this.tasks = [...this.tasks, task]
    )
    .catch(err => alert(err.message)
    )
  },

  deleteTask(id){
    if (confirm(' Are you sure?')){
      this.tasks = this.tasks.filter((task) => {
          return task.id !== id;
      })
      fetch('http://localhost:5000/tasks/'+id, {
        method: 'DELETE',
        headers: {
          'Content-Type' : 'application/json'
        }
      })
      .catch(err => {
        alert('something went wrong, /n', err.message)
        return;
    })
    }
  },


  toggleReminder(id){
    for(let i=0; i<this.tasks.length; i++){
      if(this.tasks[i].id === id){
        this.tasks[i].reminder = !this.tasks[i].reminder;
        fetch("http://localhost:5000/tasks/"+id, {
          method: 'PUT',
          headers:{
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.tasks[i]),
        })
        .then( data => {
          console.log(this.tasks[i])
          alert('Reminder changed successfully')
        }
        )
        .catch(err => 
        alert(err.message))
      }
    }

    console.log( id,"is the value of tasks after op")
  },
},

async created(){
  this.showAddTask = false;
  this.tasks = await fetch("http://localhost:5000/tasks")
  .then(res => res.json())
  .then(data => {
    return data
  })
}
}


</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
