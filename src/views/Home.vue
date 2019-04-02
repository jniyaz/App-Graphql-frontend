<template>
  <div class="home">
    <div class="mb20">
      <router-link to="books/add" class="btn-add">Add A Book</router-link>
    </div>
     <!-- Apollo Query -->
    <ApolloQuery :query="categoriesQuery">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading">Loading...</div>
        <div class="mb20" v-else>
          <a href="#" @click.prevent="selectCategory('all')" class="link-margin">All Categories</a>
          <a href="#" @click.prevent="selectCategory('featured')" class="link-margin">Featured Categories</a>
          <a href="#"
            v-for="category of data.categories"
            :key="category.id"
            @click.prevent="selectCategory(category.id)"
            class="link-margin">
            {{ category.title }}
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
            <router-link :to="`books/${book.id}`">{{ book.id }}. {{ book.title }}</router-link>
            <div><small>{{ book.author_name }}</small></div>
            <img :src="book.image" alt="cover-image">
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
              <router-link :to="`books/${book.id}`">{{ book.id }}. {{ book.title }}</router-link>
              <div><small>{{ book.author_name }}</small></div>
              <img :src="book.image" alt="cover-image">
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else>
    <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id" class="user">
            <router-link :to="`books/${book.id}`">{{ book.id }}. {{ book.title }}</router-link>
            <div><small>{{ book.author_name }}</small></div>
            <img :src="book.image" alt="cover-image">
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
import categoriesQuery from '@/graphql/queries/Categories.gql'
import booksQuery from '@/graphql/queries/Books.gql'
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'

export default {
  name: 'home',
  components: {
  },
  data() {
    return {
        categoryQuery,
        categoriesQuery,
        booksQuery,
        booksFeaturedQuery,
        selectedCategory: 'all',
        query: booksQuery,
        categories: []
    }
  },
  methods: {
    selectCategory(category) {
      if (category === 'all') {
        this.query = booksQuery
      } else if (category === 'featured') {
        this.query = booksFeaturedQuery
      } else {
        this.query = categoryQuery
      }
      this.selectedCategory = category
    }
  }
}
</script>

<style>
  .link-margin{
    margin-right: 24px;
  }
  .mb20{
    margin-bottom: 20px;
  }
  .btn-add{
    padding: 5px;
    background: #ccc;
    text-decoration: none;
  }
</style>
