<template lang="pug">
   div(class="user-banner")
      div(class="row")
        div(class="col")
          div(class="body-2 text-grey")
            | {{ repo.owner.login }}
          h4(class="mb-2")
            | {{ repo.name }}
          div(class="mb-4")
            span(
              class="label label-pill label-dark px-3 py-1 mr-2 tooltip"
              data-balloon="Watchers"
              data-balloon-pos="down"
            )
              icon( :icon="['fas', 'eye']" class="mr-2")
              | {{ repo.watchers_count }}
            span(
              class="label label-pill label-dark px-3 py-1 mr-2 tooltip"
              data-balloon="Stars"
              data-balloon-pos="down"
            )
              icon( :icon="['fas', 'star']" class="mr-2")
              | {{ repo.stargazers_count }}
          div(v-if="repo.description")
            | {{ repo.description }}
          div(v-else class="text-grey")
            | Não há descrição para este repositório.
      div(class="actions")
        button(class="button button-dark" class="github-button" @click="openOnGithub()")
          icon( :icon="['fab', 'github']" class="mr-2" class="github-button__icon" )
          | Ver no Github
</template>

<script>
export default {
  props: {
    repo: {
      type: Object,
      required: true,
    }
  },

  methods: {
    openOnGithub () {
      window.open(this.repo.html_url, '_blank');
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


.github-button {
  width: 100%;
  margin-top: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;

  &__icon {
    font-size: 1.5rem;
  }
}
</style>