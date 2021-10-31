<template lang="pug">
  div(class="wrapper" v-if="!loading")
    div(class="user-wrapper" v-if="user")
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
            div
              | {{ user.bio }}
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
    )
      template(#loading)
        div(class="loading-more text-center" v-if="loadingMore")
          i(class="spinner-grow spinner-grow-sm")
          i(class="spinner-grow spinner-grow-md")
          i(class="spinner-grow spinner-grow-sm")
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
    loadingMore: false,
    search: '',
    searchModel: '',
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

      if(this.repos.length) this.loadingMore = true;

      const username = this.$route.params.username;

      const calculatedPage = this.repos ? Math.floor(this.repos.length / 9) + 1 : 1;
      
      const params = {
        per_page: 9,
        page: calculatedPage,
      }

      const { data } = await this.$axios.get(`/users/${username}/repos`, { params });

      const reposMap = data.map(({ name, description, updated_at, html_url }) => ({
        name,
        description,
        updated_at,
        html_url
      }))

      this.loadingMore = false;
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
h6 {
  margin-bottom: 0.5rem;
  font-weight: bold;
}
.user-wrapper {
  margin-bottom: 2.5rem;
}
.user-banner {
  padding: 1.5rem;
  border: 1px solid #e3e5e6;
  border-radius: 4px;
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