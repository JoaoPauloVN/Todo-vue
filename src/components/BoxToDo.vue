<template>
  <div class="BoxToDo">
    <div class="info">
      <h3>{{ todo.name }}</h3>
      <p>{{ todo.data }}</p>
    </div>

    <button @click="Check" v-show="!editMode"><i :class="{'far fa-square': !done, 'far fa-check-square': done}"></i></button>
    <button @click="Del" v-show="editMode"><i class="fas fa-times"></i></button>
  </div>
</template>

<script>
export default {
  name: 'BoxToDo',
  props: {
    todo: Object,
    editMode: Boolean
  },
  data(){
    return {
      done: this.todo.isDone,
      id: this.todo.id
    }
  },
  methods: {
    Check(){
      this.done = !this.done
      fetch(`http://localhost:5000/tasks/${this.todo.id}`, {
        method: "PATCH",
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({'isDone': this.done})
      })
      .then(res=> res.json())
      .catch(e=> {
        return e
      })
    },
    Del() {
      let id = this.id
      fetch(`http://localhost:5000/tasks/${id}`, {
        method: "DELETE"
      })
      .then(res=> res.json())
      .then(()=> this.emitRemove(this.todo.id))

    },
    emitRemove(id){
      this.$emit('removeTodo', id)
    }
  }
}
</script>

<style scoped lang="scss">
  .BoxToDo {
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    h3 {
      color: rgb(67, 112, 126);
      font-size: 16px;
      word-break: break-all;
      width: 90%;
    }
    p {
      color: rgb(117, 153, 165);
      font-size: 14px;
      
    }
    i {
      color: rgb(36, 95, 167);
      font-size: 18px;
    }
    button {
      border: none;
      background: transparent;
      cursor: pointer;
      outline: none;
    }
  }
</style>
