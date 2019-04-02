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
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
  export default {

  }
</script>
