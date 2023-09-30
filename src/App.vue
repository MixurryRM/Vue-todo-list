<template>
  <div class="">
   <h2 class="p-3 text-center bg-primary text-white">Hello's Table</h2>
     <div class="container">
      <div class="row">
         <div class=" col mb-4 fs-3">Tasks</div>
         <div class=" col-2 mb-4 fs-3">Done</div>
      </div>

      <div class="row mb-3">
         <div class="col"><input class="form-control" v-model="newTask" type="text"></div>
         <div class="col-2 offset-1"><input type="button" v-on:click="addTask" class=" btn btn-primary" value="Add"></div>
         <div class="col-2"><input type="button" @click="deleteTasks" class="btn btn-warning" value="Delete"></div>
      </div>

     <div class="" v-if="filterTask.length > 0">
      <div class="row" v-for='(task , index) in filterTask' :key='index'>
         <div class="col" :class=" task.done ? 'delete-text' : ''" >{{ task.action }}</div>
         <div class="col-2"> 
            <input type="checkbox" v-model="task.done" />
         </div>
      </div>
   </div>

      <div class="alert alert-warning text-center" v-else >There is no tasks here!</div>
    
      <div class=" bg-danger mt-2 row py-2">
         <div class="col">Hide Complete Tasks!</div>
         <div class="col-2"><input type="checkbox" v-model="hideCompleted" /></div>
      </div>

    </div>
   </div>
</template>

<script>
export default{
    name : 'App',
    data : ()=>({
      title : 'My Table',
      newTask : '',
      hideCompleted : false,
      tasks: []
    }),
    computed : {
      filterTask(){
         return this.hideCompleted 
         ? this.tasks.filter((v)=> !v.done) 
         : this.tasks;
      },
    },
   methods: {
      addTask() {
         if (this.newTask === '') {
            return alert('YoU need to fail tasks!')
         }

         this.tasks.push({
            action: this.newTask,
            done: false
         });

         this.storeData();

         this.newTask = '';
      },

      deleteTasks() {
         this.tasks = this.tasks.filter((v) => !v.done);
         this.storeData();
      },
      
      storeData() {
         localStorage.setItem('myLocalTasks', JSON.stringify(this.tasks));
      },
   },

    mounted() {
      let data = localStorage.getItem('myLocalTasks');
      if (data !== null) {
         this.tasks = JSON.parse(data);
      }
   },
};
</script>

<style>
.delete-text {
   text-decoration: line-through;
}
</style>
