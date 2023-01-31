<template>
  <div class="container" id="app">
    <header>
      <h1>{{ name }}</h1>
    </header>

    <div class="panel">
      <input type="text" v-model="task" placeholder="Digite uma tarefa">
      <button v-on:click="addTask" class="add-task">+</button>
    </div>

    <h5 v-if="todos.length > 0">SUAS TAREFAS:</h5>

    <h5 v-if="todos.length == 0">NÃO EXISTEM TAREFAS!</h5>

    <div v-for="(todo,index ) in todos"  :key="todo.title" >
      <div class="task">
        <div v-on:click="toggleStatus(index)" class="status" v-bind:class="todo.status"></div>
        <div class="text">{{ todo.title }}</div>
        <img v-on:click="toggleModal(index)" src="@/assets/img/garbage.png" alt="Excluir" class="delete">
      </div>
    </div>

    <div class="modal-layer" v-if="showModal">
      <div class="modal">
        <h4>CONFIRMA A EXCLUSÃO?</h4>
        <p>{{ selectedTask }}</p>
        <div class="buttons">
          <button v-on:click="toggleModal()">CANCELAR</button>
          <button v-on:click="removeTask">CONFIRMAR</button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data() {
    return {
      name: 'Todo List',
        task: '',
        selectedItem: null,
        selectedTask: 'task',
        showModal: false,
        todos: [
          { title: 'Aprender VueJS', status: 'done' },
          { title: 'Aprender ReactJS', status: 'pending' },
          { title: 'Aprender NodeJS', status: 'pending' },
        ]
    }  
  },
      methods: {
        toggleModal: function (index) {
          this.showModal = !this.showModal;

          if (index !== undefined) {
            this.selectedItem = index;
            this.selectedTask = this.todos[index].title;
          }
        },
        addTask: function () {
          if (this.task !== '') {
            this.todos.push({
              title: this.task,
              status: 'pending'
            })
          }
          else if (this.task == '') {
           alert('Digite uma tarefa válida!')
          }
          this.task = '';
        },
        removeTask: function () {
          this.todos.splice(this.selectedItem, 1);
          this.showModal = false;
        },
        toggleStatus: function (index) {
          this.todos[index].status = this.todos[index].status === 'pending' ? ' done ' : 'pending';
        }        
      }
}
</script>


<style >
* {
  font-family: 'Arial';
}

html, body {
  height: 100%;
}

body {
  margin: 0;
  padding: 0;
  background: #3F3759;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  background: linear-gradient(#252041, #1d1f25);
  width: 300px;
  min-height: 600px;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  top: -60px;
  box-shadow: 10px 10px 20px 2px #000;
}

header {
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.panel {
  display: flex;
  justify-content: space-between;
}

input, .add-task {
  border: none;
  vertical-align: top;
}

input {
  padding: 10px;
  border-radius: 10px 0 0 10px;
  font-weight: bold;
  outline: none;
  width: 240px;
}

.add-task {
  background:linear-gradient(#7d2ede, #b133f7);
  font-size: 40px;
  width: 60px;
  height: 40px;
  border-radius: 0 10px 10px 0;
  line-height: 0;
  color: white;
  cursor: pointer;
}

.task {
  background: #292e3c;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px;
  border-radius: 12px;
  margin-bottom: 6px;
}

.status {
  cursor: pointer;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  margin-right: 10px;
  text-align: center;
  transition: border-color 0.5s;
}

.status.pending {
  border: 4px solid #b80404;
}

.status.done {
  border: 4px solid #10b804;
}

.text {
  flex-grow: 1;
  font-size: 16px;
  color: #fff;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 200px;
  font-weight: bold;
}

 h5 {
  display: flex;
  flex-grow: 1;
  font-size: 16px;
  color: #fff;
  padding: 5px;
  max-width: 200px;
  font-weight: bold;
}

.delete {
  width: 24px;
  height: 24px;
  cursor: pointer;
  transition: transform 0.2s;
}

.delete:hover {
  transform: rotate(16deg);
}

.modal-layer {
  position: absolute;
  width: 100%;
  min-height: 100%;
  border-radius: 10px;
  z-index: 1;
  top: 0;
  left: 0;
  background: rgba(255, 255, 255, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  font-weight: bold;
  color: #fff;
  width: 300px;
  min-height: 100px;
  border: 1px solid #292e3c;
  border-radius: 10px;
  background: linear-gradient(#252041, #1d1f25);
}

.modal h4 {
  margin: 0;
  padding: 12px;
  font-size: 14px;
  text-align: center;
  border-bottom: 1px solid white;
  word-spacing: 5px;
}

.modal p {
  font-size: 12px;
  padding: 0 10px;
  text-align: center;
  margin: 20px 0;
}

.modal .buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 10px 20px 10px;
}

.modal button {
  font-weight: bold;
  padding: 10px;
  border: none;
  border-radius: 4px;
  color: white;
  font-size: 12px;
  cursor: pointer;
}

.modal button:first-child {
  margin-right: 10px;
  background: #b80404;
}

.modal button:last-child {
  background: #0641cc;
}

.modal p {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

</style>
