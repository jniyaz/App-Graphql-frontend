<template>
  <div class="home">
     <!-- Apollo Query -->
    <ApolloQuery :query="require('@/graphql/users.gql')">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading } }">
        <!-- Some content -->
        <div v-if="loading">Loading...</div>
        <ul v-else>
          <li v-for="user of data.users" class="user">
            {{ user.name }}
          </li>
        </ul>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
import gql from 'graphql-tag'

export default {
  name: 'home',
  components: {
  },
  data() {
    return {
        categories: []
    }
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    categories: gql`query {
      categories {
        id,
        title
      }
    }`,
  },
}
</script>
