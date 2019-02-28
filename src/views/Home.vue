<template>
  <div class="home">
     <!-- Apollo Query -->
    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading } }">
        <!-- Some content -->
        <div v-if="loading">Loading...</div>
        <ul v-else>
          <li v-for="category of data.categories" :key="category.id" class="user">
            {{ category.title }}
          </li>
        </ul>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'home',
  components: {
  },
  data() {
    return {
        categories: []
    }
  }
}
</script>
