<template>
  <div class="todo__item">
    <div class="todo__item__control--left">
      <button :class="doneToggle"
        @click="() => {
          this.mutableDoneState = !this.mutableDoneState
          this.requestUpdate()
        }"
      ></button>
    </div>
    <input class="todo__item__edit"
      @keydown.enter="requestUpdate"
      @blur="requestUpdate"
      v-model="mutableMessage"
      v-if="editing"
    >
    <b
      :style="{ textDecoration: done ? 'line-through' : 'none' }"
      @click="editing = true"
      v-else
    >{{ mutableMessage }}</b>
    <div class="todo__item__control--right">
      <button class="button red" @click.prevent="$emit('delete', { id })">
        X
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo-Item',
  props: {
    id: Number,
    message: String,
    done: Boolean
  },
  data () {
    return {
      mutableMessage: '',
      mutableDoneState: false,
      editing: false
    }
  },
  computed: {
    doneToggle () {
      return this.done ? 'toggle--active' : 'toggle'
    }
  },
  created () {
    this.mutableMessage = this.message
    this.mutableDoneState = this.done
  },
  methods: {
    requestUpdate () {
      this.editing = false
      this.$emit('update', {
        id: this.id,
        message: this.mutableMessage,
        done: this.mutableDoneState
      })
    }
  }
}
</script>

<style lang="scss">
@import '@/styles/button';

.todo__item {
  width: 100%;
  height: 4rem;
  padding: 1rem;
  margin-bottom: 1.5rem;
  border-radius: 5px;
  box-shadow: 0 0 1rem rgba(0, 0, 0, .25);
  font-size: 1rem;
  line-height: 2rem;

  &:nth-last-child(1) {
    margin-bottom: 0;
  }

  &__control {
    &--left {
      float: left;
      margin-right: 1rem;
    }

    &--right {
      float: right;
      margin-left: 1rem;
    }
  }

  &__edit {
    font-size: 1rem;
  }
}
</style>
