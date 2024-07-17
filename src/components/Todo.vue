<template>
  <div class="todo">
    <p>{{ title }}</p>
    <div>
      <Btn @click="$emit('remove')" class="remove-todo-btn">&times; </Btn>
    </div>
  </div>
</template>

<script>
import Btn from "@/components/Btn.vue";
export default {
  components: { Btn },

  props: {
    title: {
      required: true,
      type: String,
    },
  },

  emits: ['remove']
};
</script>

<style scoped>
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--accent-color);
  margin-top: 30px;
  padding: 0 20px 0 20px;
  border-radius: 10px;
}

.remove-todo-btn {
  border-radius: 50%;
  border: none;
  height: 40px;
  width: 40px;
  font-size: 30px;
  color: var(--text-color);
  background: var(--danger-color);
  cursor: pointer;
}
</style>
