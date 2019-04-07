<template>
  <div class="home">
    <div class="hero bg-grey-light mb-10">
      <div class="container flex flex-col lg:flex-row lg:justify-between p-10">
        <div class="mt-10">
          <h1 class="w-full lg:w-3/4 mb-4">Book Recommendations</h1>
          <p class="leading-normal w-full lg:w-3/4 mb-6">
            Built with Laravel + Vue.js + GraphQL + Tailwind CSS
          </p>
          <div class="flex items-center">
            <a href="#" class="bg-purple-dark text-white rounded px-4 py-4 mr-4 hover:bg-purple">View Books</a>
            <a href="#" class="border border-purple-dark border-solid text-purple-dark rounded px-4 py-4 mr-4 hover:bg-purple hover:text-white">EBooks</a>
          </div>
        </div>
        <div class="mt-10 lg:mt-0">
          <img width="180px" src="https://upload.wikimedia.org/wikipedia/en/thumb/0/04/Google_Play_Books_icon_-_vector.svg/1200px-Google_Play_Books_icon_-_vector.svg.png" alt="">
        </div>
      </div>
    </div> <!-- end hero -->

    <div class="container">
      <div class="flex flex-wrap mx-2">
        <!-- Sidebar -->
        <div class="w-full lg:w-1/4 px-4 mb-8">
          <div class="px-4 mb-8">
            <ApolloQuery :query="categoriesQuery">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
                <ul v-else class="list-reset text-lg">
                  <li class="mb-4"><a href="#" @click.prevent="selectCategory('all')" class="text-black hover:text-grey-darkest">All</a></li>
                  <li class="mb-4"><a href="#" @click.prevent="selectCategory('featured')" class="text-black hover:text-grey-darkest">Featured</a></li>
                  <li class="mb-4" href="#"
                    v-for="category of data.categories"
                    :key="category.id"
                    @click.prevent="selectCategory(category.id)">
                    <a class="text-black hover:text-grey-darkest" href="#">{{ category.title }}</a>
                  </li>
                  <li class="mt-10">
                    <router-link to="books/add" class="text-black hover:text-grey-darkest border border-purple-dark border-solid text-purple-dark rounded px-2 py-1 mr-4 hover:bg-purple hover:text-white">Add A Book</router-link>
                  </li>
                </ul>
              </template>
            </ApolloQuery>
          </div>
        </div>
        <!-- Content -->
        <div class="w-full lg:w-3/4 px-4 mb-10">
          <div>
            <div v-if="selectedCategory === 'all'">
              <ApolloQuery :query="query">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                  <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
                  <div v-else class="flex flex-wrap">
                    <div v-for="book of data.books" :key="book.id" class="sm:w-1/2 lg:w-1/3 px-4 mb-12">
                      <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </template>
              </ApolloQuery>
            </div>

            <div v-else-if="selectedCategory === 'featured'">
              <ApolloQuery :query="query" :variables="{ featured: true }">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                  <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
                  <div v-else class="flex flex-wrap">
                    <div v-for="book of data.booksByFeatured" :key="book.id" class="sm:w-1/2 lg:w-1/3 px-4 mb-12">
                      <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </template>
              </ApolloQuery>
            </div>

            <div v-else>
              <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                  <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
                  <div v-else class="flex flex-wrap">
                    <div v-for="book of data.category.books" :key="book.id" class="sm:w-1/2 lg:w-1/3 px-4 mb-12">
                      <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </template>
              </ApolloQuery>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  // @ is an alias to /src
  import categoryQuery from '@/graphql/queries/Category.gql'
  import categoriesQuery from '@/graphql/queries/Categories.gql'
  import booksQuery from '@/graphql/queries/Books.gql'
  import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'
  import loadSpin from '@/components/Loader.vue'
  import bookListing from '@/components/BookListing.vue'

  export default {
    name: 'home',
    components: {
      bookListing,
      loadSpin
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
