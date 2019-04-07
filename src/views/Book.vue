<template>
  <div class="book container">
    <ApolloQuery :query="require('@/graphql/queries/Book.gql')" :variables="{ id: $route.params.id }">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
        <div v-else>
          <div class="flex mt-10 px-10 fex-col lg:flex-row">
            <div class="mt-2">
              <img :src="data.book.image" alt="cover-image">
            </div>
            <div class="w-full lg:w-3/2 ml-4 lg:ml-10">
              <div class="text-4xl font-bold mb-3">
                <small>{{ data.book.title }}</small>
              </div>
              <div class="text-xl text-grey-darkest mb-5">
                {{ data.book.description }}
              </div>
              <div class="my-12"><a target="_blank" :href="data.book.link" class="border border-purble-dark border-solid rounded text-purple p-4 hover:bg-purple hover:text-white">View Link</a></div>
               <div>
                <router-link :to="`/books/${data.book.id}/edit`">Edit</router-link> &middot;
                <a href="#" @click.prevent="deleteBook">Delete</a>
              </div>
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
  import deleteBook from '@/graphql/mutations/DeleteBook.gql'
  import loadSpin from '@/components/Loader.vue'

  export default {
    components: {
      loadSpin
    },
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
