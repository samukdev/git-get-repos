<template lang="pug">
  div(class="creator-banner mt-5 pb-4")
    span(class="text-grey body-4 mr-1")
      | Developed by
    span(
      class="body-3 creator-button"
      @click="openCreatorProfile"
    )
      | /samukdev
</template>

<script>
export default {

}
</script>

<style lang="scss">
.creator-banner {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.creator-button {
  cursor: pointer;
}
</style>