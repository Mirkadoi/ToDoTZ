<template>
<b-container fluid class="container-card">
    <b-row>
      <b-col cols="12">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
        <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
          <div class="card-properties">
            <b-card no-body :class="{completedcard: todo.completed}">
              <b-card-header class="element-container">    
                <h5 class="card-text" :class="{completedtext: todo.completed}">{{todo.title}}</h5>
                <input type="checkbox" v-model="todo.completed" > 
                </b-card-header>          
                  <b-card-body >
                    <div>
                      <pre class="card-discription-style" :class="{completedtext: todo.completed}">{{todo.discription}}</pre>
                    </div>  
                  </b-card-body>
                <b-card-footer class="footer-element-container" >
                  <b-button variant="outline-danger" class="remove-item" @click="removeTodo(index)">&times;</b-button>
                </b-card-footer>
            </b-card>
          </div>  
        </div>
         </transition-group>
      </b-col>
    </b-row>
</b-container>
</template>

<script>
export default {
  name: 'todo-list',
  data (){
    return{
      maxTitleLength: 26,
      newTodo: '',
      newTitle: '',
      beforeEditDiscription: '',
      idForTodo : 2,
      filter: 'all',
      search: '',
      todosFiltered: [],
      todos: [
        {
          'id': 1,
          'title': 'title1',
          'discription': 'text1',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },
  computed:{
    remaining (){
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining(){
      return this.remaining !=0
    },
  },

  methods: {
    removeTodo(index){
      this.todos.splice(index, 1)
      this.applyFilter()
    },
    applyFilter (filter = null) {
      let todosFiltered = [];
      if (filter !== null) {
        this.filter = filter;
      }

      if (this.filter == 'all') {
        todosFiltered = this.todos;
      } else if (this.filter == 'active') {
        todosFiltered = this.todos.filter(todo => !todo.completed);
      } else if (this.filter == 'completed') {
        todosFiltered = this.todos.filter(todo => todo.completed);
      } else {
        todosFiltered = this.todos;
      }
      
      this.todosFiltered = todosFiltered.filter((todo)=>{
        return todo.title.match(this.search);
      })

    },
  },
  mounted (){
    this.todosFiltered = this.todos;
  },
}
</script>


<style scoped>
 @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");

.container-card{
  padding:  0px;
}
.card-properties{
  margin: 8px 0px;
}
.header-title{
  margin-bottom: 0px;
}
.remove-item{
  cursor: pointer;
  padding: 2px 8px;

}
.card-text{
  padding: 10px 0px;
  margin-bottom: 0px;
}
.card-discription-style{
  font-size: 16px;
  margin-bottom: 0px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  text-align: justify;
}

.completedcard {
  background-color: rgba(109, 108, 108, 0.212);
  
}
.completedtext{
  text-decoration: line-through;
  color: rgba(109, 108, 108, 0.404);
}
.element-container {
    display: flex;
    align-items: center;
    justify-content: space-between;    
}
</style>
