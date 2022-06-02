<template>
  <div class="container">
    <h2 class="text-center mt-5">My Todo List</h2>
    <!--input-->
    <div class="d-flex">
      <input
        v-model="inputTask"
        @keyup.enter="SubmitMethod"
        type="text"
        placeholder="Enter Task"
        class="form-control"
      />
      <button @click="SubmitMethod" class="btn btn-warning rounded-0">
        Submit
      </button>
    </div>
    <!--Task table-->
    <div>
      <table class="table table-bordered">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col" class="text-center">#</th>
            <th scope="col" class="text-center">#</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>
              <textarea
                v-model="task.name"
                :disabled="task.isDisabled"
                @keyup.enter="SubmitMethod"
              >
              </textarea
              >>
            </td>
            <td>
              <!-- <select name="fruit">
                <option value="TO DO">TO DO</option>
                <option value="IN PROGRESS">IN PROGRESS</option>
                <option value="FINISHED">FINISHED</option>
              </select> -->
              <p class="text-danger" v-if="!(task.isDisabled)">PS: click on the status to edit it</p>
              <span
                @click="updateStatus(index)"
                
                :class="{
                  'text-danger': task.status === 'TO DO',
                  'text-warning': task.status === 'IN PROGRESS',
                  'text-success': task.status === 'FINISHED',
                   'not_pointer':task.isDisabled,
                   'pointer':!task.isDisabled

                }"
                >{{ task.status }}</span
              >
            </td>
            <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen"></span>
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      inputTask: "",
      //  editedTask: null,

      tasks: [
        {
          name: "my task",
          status: "IN PROGRESS",
          isDisabled: true,
        },
      ],
    };
  },
  methods: {
    SubmitMethod() {
      if (this.inputTask.length !== 0) {
        this.tasks.push({
          name: this.inputTask,
          status: "TO DO",
          isDisabled: true,
        });
        this.inputTask = "";
      }
      for (let i = 0; i < this.tasks.length; i++) {
        this.tasks[i].isDisabled = true;
        //  alert("fet");
      }
      // else{
      //   if (this.editedTask!==null){
      //     this.tasks[this.editedTask].name=this.inputTask;
      //     this.editedTask=null;
      //   }
      //   return;
      // }
    },
    deleteTask(index) {
      //  alert("fet");
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      //  this.inputTask=this.tasks[index].name;
      //this.editedTask=index;
      this.tasks[index].isDisabled = false;
    },
    updateStatus(index) {
      // alert("are you sure that u want this update?")
      if (this.tasks[index].status === "TO DO") {
        this.tasks[index].status = "IN PROGRESS";
      } else if (this.tasks[index].status === "IN PROGRESS") {
        this.tasks[index].status = "FINISHED";
      } else {
        //reset
        this.tasks[index].status = "TO DO";
      }
    },
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
.not_pointer{
    cursor: default;
}
</style>
