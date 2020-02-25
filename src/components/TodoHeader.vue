<template>
  <div class="todo__header">
    <input placeholder="To-Do item.."
      @keydown.enter="submit"
      v-model="message"
    >
    <button class="button accent" @click="submit">Add</button>
  </div>
</template>

<script>
export default {
  name: 'Todo-Header',
  data () {
    return {
      message: ''
    }
  },
  methods: {
    submit () {
      if (!this.message) {
        alert('To-Do item cannot be empty')
        return
      }
      this.$emit('insert', { message: this.message })
      this.message = ''
    }
  }
}
</script>

<style lang="scss">
@import '@/styles/button';

.todo__header {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  padding: 1rem;

  input {
    outline: none;
    flex-grow: 4;
    height: 3rem;
    border: none;
    border-radius: 5px;
    padding: 1rem;
    font-size: 1rem;
    box-shadow: 0 0 1rem rgba(0, 0, 0, .25);
  }

  button {
    flex-grow: 1;
    margin-left: 1rem;
    height: 3rem;
    box-shadow: 0 0 1rem rgba(0, 0, 0, .25);
  }
}
</style>
