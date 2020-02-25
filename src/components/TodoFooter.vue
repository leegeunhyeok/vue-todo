<template>
  <div class="todo__footer">
    <div class="todo__footer__panel">
      <div class="todo__footer__item">
        <button
          :style="{ display: filterType === filterTypes.ALL_ITEM
                    ? 'block'
                    : 'none'
          }"
          :class="checkedToggle" @click="toggleItems"
        >
        </button>
      </div>
      <div class="todo__footer__item">
        <button
          :class="isActiveFilter(filterTypes.ALL_ITEM)"
          @click="changeFilter(filterTypes.ALL_ITEM)"
        >All</button>
      </div>
      <div class="todo__footer__item">
        <button
          :class="isActiveFilter(filterTypes.DONE_ITEM)"
          @click="changeFilter(filterTypes.DONE_ITEM)"
        >Done</button>
      </div>
      <div class="todo__footer__item">
        <button
          :class="isActiveFilter(filterTypes.PENDING_ITEM)"
          @click="changeFilter(filterTypes.PENDING_ITEM)"
        >Pending</button>
      </div>
      <div class="todo__footer__item">
        <button class="button red"
          @click="$emit('deleteCheckedItems')"
        >Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import filterTypes from '@/filterTypes.js'

export default {
  name: 'Todo-Footer',
  props: {
    allChecked: Boolean,
    filterType: String
  },
  data () {
    return {
      mutableCheckedState: false,
      filterTypes
    }
  },
  computed: {
    checkedToggle () {
      return this.mutableCheckedState ? 'toggle--active' : 'toggle'
    }
  },
  watch: {
    allChecked (newVal) {
      this.mutableCheckedState = newVal
    }
  },
  created () {
    this.mutableCheckedState = this.allChecked
  },
  methods: {
    toggleItems () {
      this.$emit('toggleAllItems')
    },
    isActiveFilter (type) {
      return this.filterType === type ? 'button--active' : 'button'
    },
    changeFilter (type) {
      this.$emit('updateFilter', type)
    }
  }
}
</script>

<style lang="scss">
@import '@/styles/button';

.todo__footer {
  position: absolute;
  bottom: -5rem;
  left: 0;
  width: 100%;
  padding: 1rem;

  &__panel {
    display: flex;
    justify-content: space-around;
    height: 3rem;
    border-radius: 5px;
    padding: .5rem 1rem;
    font-size: 1rem;
    box-shadow: 0 0 1rem rgba(0, 0, 0, .25);
  }
}
</style>
