<template>
  <div class="home">

    <div class="hero bg-grey-light mb-24">
      <div class="container flex justify-between p-10">
        <div class="mt-10">
          <h1 class="w-3/4 mb-4">Book Recommendations</h1>
          <p class="leading-normal w-3/4 mb-6">
            Built with Laravel + Vue.js + Tailwind CSS
          </p>
          <div class="flex items-center">
            <a href="#" class="bg-purple-dark text-white rounded px-4 py-4 mr-4 hover:bg-purple">View Books</a>
            <a href="#" class="border border-purple-dark border-solid text-purple-dark rounded px-4 py-4 mr-4 hover:bg-purple hover:text-white">EBooks</a>
          </div>
        </div>
        <div class="pl-5">
          <img width="180px" src="https://upload.wikimedia.org/wikipedia/en/thumb/0/04/Google_Play_Books_icon_-_vector.svg/1200px-Google_Play_Books_icon_-_vector.svg.png" alt="">
        </div>
      </div>
    </div>

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

<style src="@/assets/main.css">
