<template lang="pug">
  div(class="container column")
    TheHeader(class="col-auto")
    Nuxt(class="col")
    ScrollTopButton
    TheFooter(class="col-auto")
</template>

<script>
export default {
}
</script>

<style lang="scss" scoped>
.column {
  display: flex;
  flex-direction: column;
}
</style>