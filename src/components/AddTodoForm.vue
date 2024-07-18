<template>
  <form class="add-todo-form">
    <input v-model="todoTitle" type="text" placeholder="Todo Title" />
    <div>
      <Btn :disabled="isLoading" class="btn" @click="$emit('submit', todoTitle)">
        <Spiner v-if="isLoading" /> 
        <v-else span>Add Todo</v-else>
      </Btn>
    </div>
  </form>
</template>

<script>
import Btn from './Btn.vue';
import Spiner from './Spiner.vue';

export default {
  components:{Btn, Spiner},

  props:{
    isLoading: {
      default: false,
      type: Boolean,  
    },
  },

  data() {
    return {
      todoTitle: "",
    };
  },

  emits: ["submit"],
};
</script>

<style scoped>
.add-todo-form {
  display: flex;
  justify-content: space-between;
}

.add-todo-form input {
  width: 80%;
  border: solid 2px var(--accent-color);
}

.btn{
  height: 50px;
}

</style>
