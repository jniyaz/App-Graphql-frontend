<template>
  <div class="home">
     <!-- Apollo Query -->
    <ApolloQuery :query="categoriesQuery">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a href="#" @click.prevent="selectCategory('all')" class="link-margin">All Categories</a>
          <a href="#" @click.prevent="selectCategory('featured')" class="link-margin">Featured Categories</a>
          <a href="#"
            v-for="category of data.categories"
            :key="category.id"
            @click.prevent="selectCategory(category.id)"
            class="link-margin">
            {{ category.id }}. {{ category.title }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <div v-if="selectedCategory === 'all'">
    <ApolloQuery :query="query">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.books" :key="book.id" class="user">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>
    </div>

    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="query" :variables="{ featured: true }">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.booksByFeatured" :key="book.id" class="user">
              {{ book.id }}. {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else>
    <ApolloQuery :query="categoryQuery" :variables="{ id: selectedCategory }">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div><br></div>
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id" class="user">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from '@/graphql/queries/Category.gql'
import booksQuery from '@/graphql/queries/Books.gql'
import categoriesQuery from '@/graphql/queries/Categories.gql'
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'

export default {
  name: 'home',
  components: {
  },
  data() {
    return {
        categoryQuery,
        categoriesQuery,
        booksFeaturedQuery,
        booksQuery,
        categories: [],
        selectedCategory: 'all',
        query: booksQuery,

    }
  },
  methods: {
    selectCategory (category) {
      if (category === 'all') {
        this.query = booksQuery
      } else if (category === 'featured') {
        this.query = booksFeaturedQuery
      } else {
        this.selectedCategory = category
      }
    }
  }
}
</script>

<style>
  .link-margin{
    margin-right: 24px;
  }
</style>
