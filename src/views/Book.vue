<template>
  <div class="about">
    <ApolloQuery :query="require('@/graphql/queries/Book.gql')" :variables="{ id: $route.params.id }">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div class="user">
            <img :src="data.book.image" alt="cover-image">
            <div><small>{{ data.book.author_name }}</small></div>
            <div>
              {{ data.book.description }}
            </div>
            <div>
              <router-link :to="`/books/${data.book.id}/edit`">Edit</router-link> -
              <a href="#" @click.prevent="deleteBook">Delete</a>
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
  import deleteBook from '@/graphql/mutations/DeleteBook.gql'
  export default {
    methods: {
      deleteBook () {
        this.$apollo.mutate({
          mutation: deleteBook,
          variables: {
            id: this.$route.params.id
          }
        }).then(data => {
          console.log(data)
          this.$router.push('/')
        })
      }
    }
  }
</script>
