<template lang="pug">
  div(v-if="!loading")
    div(class="row")
      div(class="col")
        RepoBanner(
          v-if="user"
          :repo="repo"
        )
      div(class="col")
        UserBanner(
          v-if="user"
          :user="user"
        )
    div(class="col-md mt-5")
      h5(class="mb-3")
        | Últimos commits
      hr(class="mt-3")

      InfiniteScroll(
        :list="commits"
        key-prop="node_id"
        class="row"
        item-class="mb-3 col-md-4"
        @loadMore="getCommits"
        :loading="loadingMoreItems"
        v-if="commits.length"
      )
        template(#item="{ item }")
          div(  
            class="card card-alternative"
          )
            div(class="card-body")
              div(class="card-title")
                div(class="ellipsis")
                  | {{ item.commit.message }}
                div(class="mt-2")
                  span(
                    class="label label-pill label-dark px-3 py-1 mr-2 tooltip"
                    data-balloon="Watchers"
                    data-balloon-pos="down"
                  )
                    icon( :icon="['fas', 'comment']" class="mr-2")
                    | {{ item.commit.comment_count }}
              div(class="card-text ellipsis-3-lines" style="min-height: 1.75rem")
                span(class="avatar avatar-sm mr-2")
                  img(:src="item.author.avatar_url")
                span
                  span
                    | {{ item.commit.author.name + ' (' +  item.author.login + ')' }}
              div(class="card-action text-right")
                button(class="button button-dark git-button" @click="openCommitOnGitHub(item.html_url)")
                  icon( :icon="['fab', 'github']" class="mr-2")
                  | Ver no GitHub
            div(class="card-footer")
              small
                | Feito em {{ formatDate(item.commit.author.date) }}
  div(v-else class="loading")
    i(class="spinner-border spinner-border-lg")
</template>

<script>
export default {
  layout: 'main',
  data: () => ({
    user: null,
    repo: null,
    loading: true,
    commits: [],
    loadingMoreItems: false,
    page: 1,
  }),

  created() {
    this.loading = true;
  },

  async mounted () {
    await this.getUser();
    await this.getRepo();
    await this.getCommits();

    this.loading = false;
  },

  methods: {
    formatDate(dt) {
      const date = new Date(dt);

      return date.toLocaleString('pt-br');
    },
    openCommitOnGitHub (url) {
      window.open(url, '_blank');
    },
    async getCommits() {
      
      const username = this.$route.params.username;
      const repoName = this.$route.params.repoName;
    
      const params = {
        per_page: 10,
        page: this.page,
      }

      this.loadingMore = true;
      const { data }  = await this.$axios.get(`/repos/${username}/${repoName}/commits`, { params})
      this.loadingMore = false;

      if(data.length > 0) {
        this.page = this.page + 1;
        this.commits.push(...data);
      }
    },
    async getUser() {
      const username = this.$route.params.username;

      const { data }  = await this.$axios.get(`/users/${username}`)

      this.user = data;
    },
    async getRepo() {
      const username = this.$route.params.username;
      const repoName = this.$route.params.repoName;

      const { data }  = await this.$axios.get(`/repos/${username}/${repoName}`)

      this.repo = data;
    }
  }
}
</script>

<style lang="scss" scoped>
.ellipsis {
  white-space: nowrap;                  
  overflow: hidden;
  text-overflow: ellipsis;
}
.loading {
  height: 100vh;
  width: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}

.git-button {
  width: 100%;
}

.card-title {
  font-weight: bold;
}

.card-alternative {
  min-height: 100%;

  &:hover {
    box-shadow: none;
  }
}
</style>