<template>
<b-container fluid>
    <b-row>
        <b-col cols="12"><input type="text" class="todo-title-input"   v-model="newTitle" placeholder="Введите заголовок задачи" :maxlength="maxTitleLength"></b-col>
        <b-col cols="12"><textarea type="text" class="todo-discription-input" v-model="newTodo" placeholder="Введите задачу"></textarea></b-col>
        <b-col cols="12"><b-button block variant="outline-success" @click="addTodo" class="btn shadow-none">Добавить</b-button></b-col>
        <b-col cols="12">
        <div class="sort-container ">
            <b-button-group size="sm">
            <b-button class="shadow-none"  :class="{ active: filter == 'all' }" @click="applyFilter('all')">Всё</b-button>
            <b-button class="shadow-none" :class="{ active: filter == 'active' }" @click="applyFilter('active')">Активные</b-button>
            <b-button class="shadow-none" :class="{ active: filter == 'completed' }" @click="applyFilter('completed')">Законченные</b-button>
            </b-button-group>
        </div>
        </b-col>
        <b-col cols="12"><div class="check-container"><div>{{ remaining }}  задач(а) не готово(а)</div>
          <div><label class="sort-label"><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Выделить все</label></div>
            </div>
        </b-col>
    </b-row>
    <ToDoList />
</b-container>
</template>

<script>
import ToDoList from './ToDoList.vue'

export default {
  name: 'todo',
  components: {
    ToDoList
  },
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
    addTodo(){
      if (this.newTodo.length == 0 || this.newTitle.length == 0 ){
        return
      }  
      this.todos.push({
        id: this.idForTodo,
        title: this.newTitle,
        discription: this.newTodo,
        completed: false,
      })
      this.newTitle = ''
      this.newTodo = ''
      this.idForTodo++
      this.applyFilter()
    },
    checkAllTodos(){
      this.todos.forEach((todo)=>todo.completed=event.target.checked)
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
.todo-discription-input,.todo-title-input,.todo-search-input {
  width: 100%;
  padding:  8px 8px;
  font-size: 16px;
  margin-bottom: 10px;
}
.sort-container {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid lightgrey;
    padding: 8px 8px;
    margin-top: 8px;   
}
.check-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    border: 1px solid lightgrey;
    padding: 8px 8px;
    margin: 8px 0px;   
}
.sort-label{
  margin-bottom:  0px;
}
</style>
