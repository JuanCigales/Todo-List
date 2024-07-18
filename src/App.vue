<template>
  <Navbar />

  <main class="container">
    <Modal v-if="editTodoForm.show" @close="editTodoForm.show = false">
      <template #header>
        <h2>Edit Todo</h2>
      </template>
      <template #content>
        <form class="edit-todo-form">
          <label>Todo Title</label>
          <input type="text" v-model="editTodoForm.todo.title" />
        </form>
      </template>
      <template #footer>
        <div class="edit-todo-modal-footer">  
          <Btn class="edit-todo-submit-btn" @click="updateTodo">Submit</Btn>
          <Btn type="danger" @click="editTodoForm.show = false">Close</Btn>
        </div>
      </template>

    </Modal>
    
    <Alert
      :message="alert.message"
      :show="alert.show"
      :type="alert.type"
      @close="alert.show = false"
    />

    <section>
      <AddTodoForm :isLoading="isPostingTodo" @submit="addTodo" />
    </section>

    <section>
      <Spiner v-if="isLoading" />
      <div v-else>
        <Todo
          v-for="todo in todos"
          :key="todo.id"
          :title="todo.title"
          @edit="showEditTodoForm(todo)"
          @remove="removeTodo(todo.id)"
        />
      </div>
    </section>
  </main>
</template>

<script>
import Alert from "./components/Alert.vue";
import Navbar from "./components/Navbar.vue";
import AddTodoForm from "./components/AddTodoForm.vue";
import Todo from "./components/Todo.vue";
import Modal from './components/Modal.vue';
import Btn from "./components/Btn.vue";
import axios from "axios";
import Spiner from "./components/Spiner.vue"

export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
    Modal,
    Btn,
    Spiner,
  },

  data() {
    return {
      todoTitle: "",
      todos: [],
      alert: {
        show: false,
        message: "",
        type: "danger",
      },

      isLoading: false,
      isPostingTodo: false,

      
      editTodoForm: {
        show: false,
        todo:{
          id:0,
          title:"",
        }
      }
    };
  },

  created(){
    this.fetchTodos();
  },

  methods: {
    async fetchTodos(){
      this.isLoading = true;
      try{
        const res = await axios.get('http://localhost:8080/todos');
        this.todos = await res.data;        
      } 
      catch(e){
        this.showAlert("Failed loading todos", "warning");   
      }
      this.isLoading = false;
    },

    showAlert(message, type = "danger"){
      this.alert.message = message;
      this.alert.type = type;
      this.alert.show =true;
    },

    async addTodo(title) {
      if (title === "") {
        this.showAlert("Todo title is required"); 
        return;
      }
      this.isPostingTodo = true;
      const res = await axios.post('http://localhost:8080/todos', {
        title
      })
      this.todos.push(res.data);
      this.alert.show = false; 
      this.isPostingTodo = false;
    },
    showEditTodoForm(todo){
      this.editTodoForm.show = true;
      this.editTodoForm.todo = {...todo};
    },
    updateTodo(){
      const todo = this.todos.find(
        (todo) => todo.id === this.editTodoForm.todo.id
      );
      todo.title = this.editTodoForm.todo.title;
      this.editTodoForm.show = false;
    },
    async removeTodo(id) {
      await axios.delete(`http://localhost:8080/todos/${id}`);
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
  },
};
</script>
<style scoped>
.edit-todo-form > input {
  width: 100%;
  height: 30px;
  border: 1px solid var(--accent-color);
}

.edit-todo-modal-footer {
  display: flex;
  justify-content: end;
  padding: 10px;
}

.edit-todo-submit-btn {
  margin-right: 5px;
}
</style>