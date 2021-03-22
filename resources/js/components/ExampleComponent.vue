<template>
    <div class="container">
        <h1>My Tasks</h1>
        <div class="pt-2">
            <h4>New Task</h4>
        </div>
        <form action="#" @submit.prevent="createTask()" method="post">
            <div class="col-md-9">
                <input type="text" v-model="task.body" class="form-control">
                <span :key="index" class="text-danger" v-for="(msg,index) in errors.body">
                        {{ msg }}
                </span>
            </div>
            <div class="col-md-3">
                <button type="submit" class="btn btn-primary">New Task</button>
            </div>
        </form>
        <div style="padding-top: 50px;">
            <h4>All Tasks</h4>
        </div>
        <div class="col-md-10">
            <ul class="list-group">
                <li v-if="list.length==0">There are no tasks yet!!</li>
                <li :key="task.id" class="list-group-item" v-for="(task) in list">
                    {{task.body}}
                    <button @click="deleteTask(task.id)" class="btn btn-danger btn-xs pull-right">Delete</button>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                list: [],
                task: {
                    id: '',
                    body: ''
                },
                errors: [],
            };
        },
        created() {
            this.fetchTask();
        },
        methods:{
            fetchTask() {
                axios.get('api/tasks').then(res => {
                    this.list = res.data;
                });
            },
            createTask() {
                axios.post('api/tasks', this.task).then(res => {
                    this.task.body='';
                    this.edit=false;
                    this.fetchTask();
                }).catch(error => {
                       console.log(error.response.data)
                       
                       this.errors = error.response.data.error
                       
                     });
            },
            deleteTask(id) {
                axios.delete('api/tasks/'+id).then(res => {
                    this.fetchTask();
                }).catch((err) => console.error(err));
            }
        }
    }
</script>
