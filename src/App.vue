<template>
  <div id="app">
    <TodoHeader @insert="addTodoItem"/>
    <div class="todo">
      <TodoItem v-for="item in filteredTodos"
        @update="updateTodoItem"
        @delete="deleteTodoItem"
        :id="item.id"
        :message="item.message"
        :done="item.done"
        :key="item.id"
      />
      <div class="todo--empty" v-if="filteredTodos.length === 0">
        To-Do Item is empty..
      </div>
    </div>
    <TodoFooter
      @toggleAllItems="toggleAllItems"
      @updateFilter="updateFilter"
      @deleteCheckedItems="deleteCheckedItems"
      :allChecked="isAllDone"
      :filterType="currentFilter"
    />
    <div class="source-code">
      <h6>Using Vue</h6>
      <a href="https://github.com/leegeunhyeok/vue-todo">Source Code</a>
    </div>
    <div class="checkout">
      <a class="react"
        href="https://docs.geundung.dev/pages/react-todo"
      >React</a>
      <a class="vue active"
        href="https://docs.geundung.dev/pages/vue-todo"
      >Vue</a>
      <a class="angular" href="./?">Angluar</a>
    </div>
  </div>
</template>

<script>
import TodoStorage from '@/TodoStorage'
import TodoHeader from '@/components/TodoHeader.vue'
import TodoItem from '@/components/TodoItem.vue'
import TodoFooter from '@/components/TodoFooter.vue'

import filterTypes from '@/filterTypes.js'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoItem,
    TodoFooter
  },
  data () {
    return {
      storage: null,
      todos: [],
      currentFilter: filterTypes.ALL_ITEM
    }
  },
  computed: {
    isAllDone () {
      return !!this.todos.length && this.todos.every(t => t.done)
    },
    filteredTodos () {
      let filteredTodo
      switch (this.currentFilter) {
        case filterTypes.ALL_ITEM:
          filteredTodo = this.todos
          break

        case filterTypes.DONE_ITEM:
          filteredTodo = this.todos.filter(todo => todo.done)
          break

        case filterTypes.PENDING_ITEM:
          filteredTodo = this.todos.filter(todo => !todo.done)
          break

        default:
          filteredTodo = this.todos
      }

      return filteredTodo
    }
  },
  created () {
    const todoStorage = new TodoStorage('vue-todo_v1')
    this.todos = todoStorage.select()
    this.storage = todoStorage
  },
  methods: {
    getTodoItem () {
      this.todos = this.storage.select()
    },
    addTodoItem ({ message }) {
      this.storage.insert(message)
      this.getTodoItem()
    },
    updateTodoItem ({ id, message, done }) {
      this.storage.update(id, message, done)
      this.getTodoItem()
    },
    deleteTodoItem ({ id }) {
      this.storage.delete(id)
      this.getTodoItem()
    },
    toggleAllItems () {
      const done = !this.isAllDone
      this.todos.forEach(todo => {
        this.storage.update(todo.id, todo.message, done)
        this.getTodoItem()
      })
    },
    updateFilter (type) {
      this.currentFilter = type
    },
    deleteCheckedItems () {
      if (!confirm('Are you sure?')) {
        return
      }

      this.todos
        .filter(todo => todo.done)
        .forEach(todo => {
          this.storage.delete(todo.id)
          this.getTodoItem()
        })
    }
  }
}
</script>

<style lang="scss">
html, body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
}

* {
  font-family: system-ui;
  box-sizing: border-box;
}

#app {
  position: relative;
  width: 100%;
  max-width: 400px;
  max-height: 90vh;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.todo {
  margin-top: 5rem;
  max-height: 40vh;
  padding: 1rem;
  overflow-y: auto;

  &--empty {
    color: #777;
    text-align: center;
  }
}

.source-code {
  text-align: center;

  h6 {
    margin: 5px 0;
  }

  a {
    color: #777;

    &:visited {
      color: #777;
    }
  }
}

.checkout {
  margin-top: .5rem;
  text-align: center;

  a {
    margin: 0 .5rem;
    text-decoration: none;
    font-weight: bold;

    &.active {
      text-decoration: underline;
    }

    &.react, &.react:visited {
      color: #61dbfb;
    }

    &.vue, &.vue:visited{
      color: #41b883;
    }

    &.angular, &.angular:visited {
      color: #b52e31;
    }
  }
}
</style>
