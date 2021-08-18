<template>
  <div class="ModalTodo" @click="closeModal">
      <div class="box_modal" @click.stop>
          <h2>Add Todo</h2>
          <input type="text" v-model="todoName" name="todoName" autocomplete="off">
          <button @click="addTodo">asdas</button>
      </div>
  </div>
</template>

<script>
export default {
  name: 'ModalTodo',
  data(){
      return {
          todoName: ''
      }
  },
  methods: {
    addTodo(){
        if(this.todoName.length){
            fetch(`http://localhost:5000/tasks`, {
                    method: "POST",
                    headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                id: Math.floor(Math.random() * 1000),
                name: this.todoName,
                data: Date(Date.now()).slice(0, 24),
                isDone: false
            })
            })
            .then(res=> res.json())
            .then(res=> {
                this.$emit('sendTodo', res)
                this.todoName = ''
            })
            .catch(e=> {
                return e
            })
        }
    }, 
    closeModal(){
        this.$emit('closeModal')
    }
  }
}
</script>

<style scoped lang="scss">
    .ModalTodo {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        background: rgba(181, 221, 228, 0.4);
        display: flex;
        align-items: center;
        justify-content: center;
        .box_modal {
            background: white;
            width: 300px;
            padding: 15px;
            border-radius: 10px;
            justify-content: center;
            display: flex;
            flex-wrap: wrap;
            h2 {
                color: rgb(37, 76, 83);
                font-size: 20px;
                margin-bottom: 10px;
            }
            input {
                width: 100%;
                border: none;
                box-shadow: -5px 5px 30px -8px rgba(32, 160, 182, 0.6);
                padding: 10px 15px;
                border-radius: 20px;
                outline: none;
                color: rgb(22, 69, 76);
            }
            button {
                margin-top: 15px;
                padding: 10px 20px;
                border-radius: 20px;
                border: none;
                background: linear-gradient(to right, rgb(109, 200, 214), rgb(73, 48, 218));
                color: white;
            }
        }
    }
</style>
