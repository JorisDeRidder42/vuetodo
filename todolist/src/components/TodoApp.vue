<template>
  <div class="container">
    <h2 class="text-center my-5">My vue {{naam}}</h2>


    <div class="d-flex">
      <input v-model="task" type="text" placeholder="enter a task" class="form-control">
      <button @click="submitTask" class="btn btn-primary">SUBMIT</button>
    </div>

    <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task,index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'finished'}">
          {{task.name}}
          </span>
      </td>

      <td class="breedteColumn">
          <span   @click="changeStatus(index)" class="pointer"  
            :class="{'text-danger' : task.status === 'to-do',
            'text-warning' : task.status === 'in-progress',
            'text-success' : task.status === 'finished'}"
            >
            {{firstCharUpper(task.status)}}
          </span>
        </td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fas fa-pen"></span>
        </div>
      </td>
      <td>
      <div  class="text-center" @click="deleteTask(index)">
        <span class="fas fa-trash"></span>
      </div>
      </td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
export default {
  data(){
    return{
      task: '',
      naam: 'to-do-list app',
      editedTask: null,
      avaiableStatuses:['to-do', 'in-progress', 'finished'],
      tasks:[
        {
          name: 'Make a todo list on vue',
          status: 'to-do'
        },
         {
          name: 'Take a 20 minutes break',
          status: 'in-progress'
        },
      ]
    }
  },

  methods:{
    submitTask(){
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
         name: this.task,
         status: 'to-do',
       });
      }
      else{
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null;
      }

       this.task = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
    },
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.avaiableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.avaiableStatuses[newIndex];
    },
    firstCharUpper(str){ 
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<style>
.pointer{
  cursor: pointer;
  }
  .breedteColumn{
    width:120px;
  }
  .finished{
    text-decoration: line-through;
  }
</style>
