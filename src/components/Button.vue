<template>
  <button
    type="button"
    class="btn dropdown-toggle d-flex align-items-center justify-content-between"
    @click="onClick"
  >
    <slot></slot>
  </button>
</template>

<script lang="ts">
/**
 * Imports
 */
import { toRefs, defineComponent } from 'vue'

export default defineComponent({
  /**
   * Props
   */
  props: {
    children: {
      type: String,
      require: true,
      default: () => ''
    }
  },
  /**
   * Emits methods
   */
  emits: ['click'],
  setup(props, context) {
    /**
     *  Refs
     */
    const { children } = toRefs(props)

    /**
     *  Funtions
     */
    const onClick = () => {
      context.emit('click')
    }

    return { children, onClick }
  }
})
</script>

<style scoped>
.btn {
  width: 126px;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  align-items: center;
  padding: 9px 16px;
}
</style>
