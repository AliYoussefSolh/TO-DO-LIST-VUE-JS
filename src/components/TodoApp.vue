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
                        <th scope="col" class="text-center">Priority</th>
                        <th scope="col">Task</th>
                        <th scope="col">Status</th>
                        <th scope="col" class="text-center">#</th>
                        <th scope="col" class="text-center">#</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in tasks" :key="index">
                    <td class="number_pr"> <input type="number" v-model="task.priority" :disabled="task.isDisabled"  @keydown.ctrl.83.prevent.stop="SubmitMethod" > </td>
                        <td>
                            <textarea class="task_txt"
                                v-model="task.name"
                                :disabled="task.isDisabled"
                                 @keydown.ctrl.83.prevent.stop="SubmitMethod"
                            >
                            </textarea
                            >>
                            <p class="text-danger" v-if="!task.isDisabled">
                                PS: click ctrl +S to save changes
                            </p>
                        </td>
                        <td class="number_pr">
                    
                            <p class="text-danger" v-if="!task.isDisabled">
                                PS: click on the status to edit it
                            </p>
                            <span
                                @click="updateStatus(index)"
                                @keydown.ctrl.83.prevent.stop="SubmitMethod"
                                :class="{
                                    'text-danger': task.status === 'TO DO',
                                    'text-warning':
                                        task.status === 'IN PROGRESS',
                                    'text-success': task.status === 'FINISHED',
                                    pointer: !task.isDisabled,
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
    name: "TodoApp",

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
                    priority:1
                    
                }
            ],
            numberOfTasks:1
        }
    },
    methods: {
        SubmitMethod() {
            
            if (this.inputTask.length !== 0) {
                this.numberOfTasks++;
                
                this.tasks.push({
                    name: this.inputTask,
                    status: "TO DO",
                    isDisabled: true,
                    priority:this.numberOfTasks
                });
                this.inputTask = "";
                
                
            }
            for (let i = 0; i < this.tasks.length; i++) {
                this.tasks[i].isDisabled = true;
                //  alert("fet");
            }
            this.sortTasks();
           
        },
        deleteTask(index) {
            //  alert("fet");
            this.tasks.splice(index, 1);
            this.numberOfTasks--;
        },
        editTask(index) {
            //  this.inputTask=this.tasks[index].name;
            //this.editedTask=index;
            this.tasks[index].isDisabled = false;
        },
        
        updateStatus(index) {
            if (this.tasks[index].isDisabled === false) {
                if (this.tasks[index].status === "TO DO") {
                    this.tasks[index].status = "IN PROGRESS";
                } else if (this.tasks[index].status === "IN PROGRESS") {
                    this.tasks[index].status = "FINISHED";
                } else {
                    //reset
                    this.tasks[index].status = "TO DO";
                }
            }
            // alert("are you sure that u want this update?")
        },
        sortTasks(){
            this.tasks.sort(function(a,b){
                return a.priority-b.priority;
            });
        }
        
        
    }
   
};
</script>
<style scoped>
.pointer {
    cursor: pointer;
}
.task_txt {
    width: 100%;
    height: 100%;
    resize: none; 
     -webkit-box-sizing: border-box; 
    -moz-box-sizing: border-box; 
    box-sizing: border-box; 
}
.number_pr{
    width: 200px;

}
</style>
