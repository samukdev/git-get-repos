<template lang="pug">
  div(class="mb-5")
    div(class="row")
      div(class="col")
        button(
          class="button button-light button-sm"
          @click="$router.go(-1)"
        )
          icon(
            :icon="['fas', 'arrow-left']"
            class="mr-2"
          )
          | Voltar
      div(class="col-auto text-right")
        div(class="home-button" class="tooltip" data-balloon="Home" data-balloon-pos="down")
          icon(
            :icon="['fab', 'github-alt']"
            @click="$router.push('/')"
        )
</template>

<script>
export default {
  methods: {
    openCreatorProfile() {
      window.open('https://samukdev.github.io', '_blank')
    }
  }
}
</script>

<style lang="scss" scoped>
  .home-button {
    font-size: 2rem;
    transition: .2s all;
    display: flex;
    align-items: center;

    span {
      font-size: 0.9rem;
      margin-right: 0.5rem;
    }
    &:hover {
      transform: scale(1.05);
      cursor: pointer;
    }
  }
  .row {
    align-items: center;
  }
  .body-3 {
    font-weight: bold;
  }
</style>