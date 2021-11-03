<template lang="pug">
  transition(name="fade")
    button(
      v-if="visible"
      @click="scrollToTop"
    )
      icon(
        :icon="['fas', 'chevron-up']"
      )
</template>

<script>
export default {
  name: 'ScrollTopButton',
  data: () => ({
    visible: false,
  }),

  mounted() {
    window.addEventListener('scroll', this.handleScroll);
  },

  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll);
  },

  methods: {
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },

    handleScroll() {
      const offset = window.pageYOffset;

      this.visible = offset > 75;
    },
  },

};
</script>

<style lang="scss" scoped>
@import "~empiricus-css/src/assets/scss/vars.scss";
button {
  position: fixed;
  bottom: 1rem;
  right: 1rem;

  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  border: none;
  z-index: 2;
  background: $primary;
  font-size: 1.25rem;
  color: #f3f3f3;
}

</style>
