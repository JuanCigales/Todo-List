<template>
    <button :style="{backgroundColor}" @click.prevent="$emit('click')">
        <slot />
    </button>
</template>

<script>
export default{
    props:{
        type:{
            default: "danger",
            validator(value){
                return ["danger", "warning", "info"].includes(value);
            },
        },
    },

    computed:{
        backgroundColor(){
            const options = {
                danger: "var(--danger-color)",
                warning: "var(--warning-color)",
                info: "var(--info-color)",
            }
            return options[this.type];
        },
    },

    emits: ['click'],
};

</script>

<style scoped>

.add-todo-form button {
  background: var(--accent-color);
  color: var(--text-color);
  border: none;
  height: 50px;
}

</style>