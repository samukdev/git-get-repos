<template lang="pug">
  div
    div(
      :class="itemClass"
      v-for="item in list"
      :key="item[keyProp]"
    )
      slot(name="item" v-bind:item="item")
    div(v-if="list.length" v-observe-visibility="handleScrolledToBottom")
    slot(name="loading")
</template>

<script>
export default {
  props: {
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

<style>

</style>