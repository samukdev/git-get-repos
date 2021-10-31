<template lang="pug">
  div
    div(
      :class="itemClass"
      v-for="item in list"
      :key="item[keyProp]"
    )
      slot(name="item" v-bind:item="item")
    div(v-if="list.length" v-observe-visibility="handleScrolledToBottom")
    div(class="loading-more text-center" v-if="loading")
      i(class="spinner-grow spinner-grow-sm")
      i(class="spinner-grow spinner-grow-md")
      i(class="spinner-grow spinner-grow-sm")
</template>

<script>
export default {
  props: {
    loading: {
      type: Boolean,
      default: false,
    },
    list: {
      type: Array,
      default: () => [],
      required: true,
    },
    keyProp: {
      type: String,
      default: 'id',
      required: false,
    },
    itemClass: {
      type: String,
      default: '',
    }
  },

  data: () => ({
    isVisible: true,
  }),

  methods: {
    handleScrolledToBottom (isVisible) {
      if(!isVisible) { return }

      this.$emit('loadMore')
    }
  }
}
</script>

<style lang="scss" scoped>
.loading-more {
  width: 100%;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;

  i {
    margin: 0.5rem;
  }
}
</style>