<template>
  <div class="font-serif mt-5 mb-32">
    <h1 class="lg:mt-10 lg:text-xl lg:font-bold lg:text-blue-900 font-bold">Todo List</h1>
    <div class="lg:bg-zinc-200 lg:shadow-md lg:rounded-lg lg:mt-4 lg:mr-96 lg:ml-96 text-center justify-center align-middle lg:pl-32 lg:py-20 mr-5 ml-4 shadow-md">
      <div class="lg:flex lg:mb-4 mb-2 ">
        <input v-model="task" type="text" placeholder="enter task"
          class="lg:mr-2 lg:h-10 lg:rounded-md lg:w-96 lg:outline-none lg:placeholder:md lg:py-6 lg:-mt-2 lg:-ml-20 outline-none mb-4 w-full py-3 px-20 bg-zinc-100 focus:border-cyan-500 lg:bg-white rounded-md mt-6">
        <input v-model="date" type="date"
          class="lg:outline-none lg:rounded-md outline-none mb-4 ml-6 py-4 px-8 lg:py-3 lg:-mt-2 lg:px-3">
        <button @click="addText"
          class="bg-cyan-800 lg:rounded-md lg:text-black lg:px-4 lg:-mt-2 lg:ml-4 text-white rounded-md py-2 px-12 ml-2 mb-4 lg:bg-white lg:hover:scale-125 ">Add Task</button>
      </div>
      <div>
        <div v-for="(task, index) in tasks" :key="index"
          class="lg:flex lg:bg-white lg:mr-10 lg:py-3 lg:mb-2 lg:-ml-24 lg:pl-5 shadow-md bg-zinc-200 mb-2">
          <div class="w-full lg:flex lg:mt-2">
            <p class="lg:-ml-5 lg:w-1/3">
              <span :class="{'finished': task.status === 'done'}">{{ task.title }}</span>
            </p>
            <p class="lg:ml-20 lg:mr-20 lg:w-1/5">{{ task.date }}</p>
            <span @click="chnageStatus(index)" class="cursor-pointer lg:ml-4 lg:w-1/5"
            :class="{'text-green-600' :task.status ==='done',
                     'text-yellow-300' :task.status ==='in progress',
                     'text-red-600' :task.status === 'To-Do'
            }"
            >{{ task.status }}</span>
          </div>
          <div class="pb-4 pt-4 lg:-mt-2 lg:w-1/4 ml-4">
            <i @click="deleteTask(index)" class="pi pi-trash lg:text-red-600 text-red-600 mr-10 bold"></i>
            <i @click="editTask(index)" class="pi pi-pen-to-square text-zinc-500 mr-10"></i>  
            <i v-if="complete" class="pi pi-check text-green-500 text-lg font-bold ml-10 mt-4"></i>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatus: ['To-Do', 'in progress', 'done'],
      tasks: [
        {
          title: 'something to do...',
          date: new Date().toISOString().slice(0, 10),
          status: 'To-Do'
        }
      ],
      complete: false,
      newText: '',
      date: '',
      calander: '',
      taskID: null,
    }
  },
  mounted() {
    const storedTasks = JSON.parse(localStorage.getItem('tasks'));
    if (storedTasks) {
      this.tasks = storedTasks;
    }
  },
  methods: {
    addText() {
      if (this.editedTask === null) {
        this.tasks.push({
          title: this.task,
          date: this.date,
          status: 'To-Do'
        });
      } else {
        this.tasks[this.editedTask].title = this.task;
        this.tasks[this.editedTask].date = this.date;
        this.editedTask = null; 
      }

      this.saveTaskToLocalStorage(); 


      this.task = '';
      this.date = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTaskToLocalStorage(); 
    },
    editTask(index) {
      this.task = this.tasks[index].title;
      this.date = this.tasks[index].date;
      this.editedTask = index;
    },
    chnageStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
      this.saveTaskToLocalStorage(); 
    },
    saveTaskToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  },
}
</script>

<style>
body{
  background-color: rgb(12, 3, 35);
}
.finished {
  text-decoration: line-through;
}
</style>