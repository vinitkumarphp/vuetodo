<template>
<div class="w-50">

<form @submit.prevent="saveData" >
    <div class="input-group  w-100">

  <input type="text" v-model="form.title" :class="{'is-invalid' : form.errors.has('title')}" class="form-control form-control-lg" placeholder="Add a Task" aria-label="Recipient's username" aria-describedby="button-addon2">
  <button class="btn btn-success" type="submit" id="button-addon2">Add</button>

</div>
    <span class="text-danger pt-3 pb-3" style="font-size:20px;" v-if="form.errors.has('title')" v-text="form.errors.get('title')"></span>
</form>
<ul class="list-group">
    <li class="list-group-item list-group-item-action" v-for="todo in todos" :key="todo.id"  >{{todo.title}}</li>
</ul>

</div>
</template>

<script>
    export default {
data(){
            return{
                editmode: false,
                todos:'',
                form: new Form({
                    title: '',
                })
            }
        },
        methods:{
            saveData(){
                //console.log(this.form.title);
                var data=new FormData();
                data.append('title',this.form.title);
                axios.post('./api/todo/', data).then((res) =>{
                    this.todos = res.data;
                    this.getTodos();
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                })
            },

        getTodos(){
                    axios.get('./api/todo').then((res) =>{
                        this.todos = res.data
                    }).catch((error) =>{
                        console.log(error)
                    })
            },
    },
        mounted() {
            this.getTodos();
        }
    }
</script>
