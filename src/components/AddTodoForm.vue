<template>
  <form class="add-todo-form">
    <input v-model="todoTitle" type="text" placeholder="Todo Title" />
    <div>
      <Btn @click.prevent="$emit('submit', todoTitle)">Add Todo</Btn>
    </div>
  </form>
</template>

<script>
import Btn from './Btn.vue';

export default {
  components:{Btn},
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

</style>
