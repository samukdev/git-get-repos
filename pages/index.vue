<template lang="pug">
  main(class="wrapper")
    section
      div
        img(src="github.png" class="git-logo")
        h5
          | Pesquisa rápida por repositórios a partir de um usuário GitHub.
      form(@submit.prevent="getUser(searchText)")
        input(
          type="text"
          class="input"
          placeholder="Nome de usuário"
          v-model.trim="searchText"
          :class="{ 'is-invalid': error }"
          @input="error = null"
        )
        small(v-show="error" class="form-text text-red")
          | {{ error }}
        button(class="button button-primary" type="submit" :class="{ 'is-loading': isLoading }")
          | Buscar
</template>

<script>
export default {
  data: () => ({
    searchText: '',
    isLoading: false,
    error: false,
  }),

  methods: {
    getUser (user) {
      this.error = false;
      this.isLoading = true;

      this.$axios.get(`/users/${user}`).then(() => {
        this.$router.push(`/user/${user}`)
      }).catch(err => {
        if(err.response.status === 404) {
          this.error = 'Poxa, parece que esse usuário não existe :('
        } else {
          this.error = 'Algo deu errado, tente novamente mais tarde.'
        }
      }).finally(() => {   
        this.isLoading = false;
      })


    }
  }
}
</script>

<style lang="scss" scoped>
.git-logo {
  margin-bottom: 2rem;
}
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 90vh;
  width: 100%;

  .avatar {
    margin: 0 auto;
    margin-bottom: 2rem
  }

  section {
    display: flex;
    flex-direction: column;
    padding: 1rem;
    width: 100%;
    max-width: 500px;
    text-align: center;

    h5 {
      font-weight: 600;
      color: #333333;
    }

    form {
      width: 100%;
      max-width: 320px;
      margin: 0 auto;

      input {
        width: 100%;
        margin: 0 auto;
      }
  
      button {
        box-shadow:  0 4px 14px 0 rgba(236, 78, 78, 0.35);
        width: 100%;
        margin: 0 auto;
        margin-top: 1.5rem;
        transition: .2s all;
  
        &:active {
          transform: scale(0.98)
        }
      }
    }
  }
}
</style>
