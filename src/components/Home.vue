<template>
  <div class="main">
    <div class="box_todo">
      <div class="header">
        <h2>Todo List</h2>
        <i class="fas fa-ellipsis-v" @click="EditMode"></i>
      </div>
      <hr>
      <div class="box_todos">
        <BoxToDo v-for="todo in todos" :key="todo.id" :todo="todo" :editMode="editMode" @removeTodo="RemoveTodo"/>
      </div>
      <ModalTodo v-show="modal" @sendTodo="addTodos" @closeModal="CloseModal"/>
      
    </div>
    <button @click="newTodo" class="btnModal" :class="{active: modal}"><i class="fas fa-times"></i></button>
  </div>
</template>

<script>
import BoxToDo from '@/components/BoxToDo.vue'
import ModalTodo from '@/components/ModalTodo.vue'

export default {
  name: 'Home',
  data(){
    return {
      todos: [],
      modal: false,
      editMode: false
    }
  },
  components: {
    BoxToDo,
    ModalTodo
  },
  methods: {
    newTodo(){
      this.modal = true
      let button = document.querySelector('.btnModal')
      if(button.classList.contains('active')) {
        this.modal = false
      }
    }, 
    addTodos(res){
      this.todos.push(res)
      this.modal = false
    },
    CloseModal(){
       this.modal = false
    },
    EditMode(){
      this.editMode = !this.editMode
    },
    RemoveTodo(id){
      let newTodo = this.todos.filter(todo=> {
        if(todo.id !== id)       
          return true
        else
          return false
      })
      this.todos = newTodo
    }
  },
  created() {
    fetch('http://localhost:5000/tasks/')
        .then(res=>{ return res.json()})
        .then(res=>{ this.todos = res})
  }
}
</script>

<style lang="scss" scoped>
  .main {
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgb(231, 239, 240);
    .box_todo {
      background: white;
      width: 400px;
      border: 1px solid rgb(74, 164, 175);
      padding: 20px 10px;
      border-radius: 10px;
      box-shadow: 0 5px 18px 1px rgba(74, 164, 175, 0.6);
      .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        h2 {
          color: rgb(37, 76, 83);
          font-size: 20px
        }
        i {
          margin-right: 10px;
          font-size: 18px;
          color: rgb(37, 76, 83);
          cursor: pointer;
        }
      }
      hr { 
        margin: 10px 0;
        border: 1px solid rgb(234, 242, 243);
      }
    }
    button {
      position: fixed;
      display: flex;
      align-items: center;
      justify-content: center;
      bottom: 40px;
      right: 40px;
      width: 60px;
      height: 60px;
      border: none;
      outline: none;
      cursor: pointer;
      border-radius: 50%;
      background: linear-gradient(to right, rgb(109, 200, 214), rgb(73, 48, 218));
      color: white;
      font-weight: bold;
      font-size: 27px;
      transition: 200ms;
      transform: rotate(45deg);
      &.active {
        transform: rotate(180deg);
      }
    }
  }
</style>
