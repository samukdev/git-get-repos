<template lang="pug">
   div(class="user-banner")
      div(class="row")
        div(class="col-auto")
          div(class="avatar avatar-xl")
            img(
              :src="user.avatar_url"
            )
        div(class="col")
          div(class="body-3")
            | {{ user.login }}
          h6
            | {{ user.name }}
          div
            | {{ user.company }}
          div(v-if="user.bio")
            | {{ user.bio }}
          div(v-else class="text-grey")
            | Este usuário não possui biografia.

</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style lang="scss" scoped>
h6 {
  margin-bottom: 0.5rem;
  font-weight: bold;
}

.user-banner {
  padding: 1.5rem;
  border: 1px solid #e3e5e6;
  border-radius: 4px;
}
</style>