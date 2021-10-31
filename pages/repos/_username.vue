<template lang="pug">
  div(class="wrapper" v-if="!loading")
    div(class="user-banner-wrapper" v-if="user")
      UserBanner(
        :user="user"
      )

    div
      h4
        | Repositórios
        span
          | ({{ user.public_repos }})

    InfiniteScroll(
      :list="repos"
      key-prop="name"
      class="row"
      item-class="col-md-4 mb-4"
      @loadMore="getRepos"
      :loading="loadingMoreItems"
      v-if="repos.length"
    )
      template(#item="{ item }")
        div(  
          class="card card-alternative"
        )
          div(class="card-body")
            div(class="card-title ellipsis")
              | {{ item.name }}
            div(class="card-text ellipsis-3-lines" style="min-height: 1.75rem")
              | {{ item.description || '' }}
            div(class="card-action")
              button(class="button button-sm button-dark" @click="openInNewTab(item.html_url)")
                | Ver no GitHub
              button(class="button button-sm button-primary" @click="$router.push({ name: 'commits', params: { repo_id: 12 } })")
                | Detalhes
          div(class="card-footer")
            small
              | Última atualização em {{ formatDate(item.updated_at) }}
    div(v-else class="mt-5 text-center text-grey")
      | Este usuário não possui repositórios públicos no momento :(
  div(v-else class="loading")
    i(class="spinner-border spinner-border-lg")
</template>

<script>
/* eslint-disable camelcase */

export default {
  data: () => ({
    repos: [],
    user: null,
    loading: true,
    loadingMoreItems: false,
  }),

  created() {
    this.loading = true;
  },

  async mounted () {
    await this.getUser();
    await this.getRepos();

    this.loading = false;
  },

  methods: {
    openInNewTab(url) {
      window.open(url, '_blank');
    },

    formatDate(dt) {
      const date = new Date(dt);

      return date.toLocaleString('pt-br');
    },
    async getRepos () {
      if(this.repos.length >= this.user.public_repos) return

      if(this.repos.length) this.loadingMoreItems = true;

      const username = this.$route.params.username;

      const calculatedPage = this.repos ? Math.floor(this.repos.length / 12) + 1 : 1;
      
      const params = {
        per_page: 12,
        page: calculatedPage,
      }

      const { data } = await this.$axios.get(`/users/${username}/repos`, { params });

      const reposMap = data.map(({ name, description, updated_at, html_url }) => ({
        name,
        description,
        updated_at,
        html_url
      }))

      this.loadingMoreItems = false;
      this.repos.push(...reposMap);

    },

    async getUser() {
      const username = this.$route.params.username;

      const { data }  = await this.$axios.get(`/users/${username}`)

      this.user = data;
    }
  },
}
</script>

<style scoped lang="scss">
.loading {
  height: 100vh;
  width: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}

h4 {
  padding: 0 1rem;
}

h4 span {
  font-size: 1rem;
  height: 100%;
  color: #606060;
  margin-left: 0.25rem;
}

.user-banner-wrapper {
  margin-bottom: 2.5rem;
}

.ellipsis {
  white-space: nowrap;                  
  overflow: hidden;
  text-overflow: ellipsis;
}
.ellipsis-3-lines {
  white-space: nowrap;                  
  overflow: hidden;
  text-overflow: ellipsis;
}
.card-action > button:first-of-type {
  margin-right: 0.5rem;
}
.card-wrapper {
  margin-bottom: 1.5rem;
}
.wrapper {
  max-width: 1400px;
  margin: 0 auto;
  padding: 3rem 1rem;
}

.card-alternative {
  min-height: 100%;

  &:hover {
    box-shadow: none;
  }

  &:not(:last-child) {
    margin-bottom: 1rem;
  }
}

</style>