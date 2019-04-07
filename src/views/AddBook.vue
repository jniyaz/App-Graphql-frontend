<template>
  <div class="AddBook container">
    <div class="mt-10 mb-10 px-10">
        <form id="contact" class="w-full max-w-md" action="" method="post" @submit.prevent="addBook">
          <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-book-name">
                Book Name
              </label>
              <input v-model="title" placeholder="Book Name" type="text" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" id="grid-book-name" tabindex="1" required autofocus>
            </div>
            <div class="w-full md:w-1/2 px-3">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-author-name">
                Author Name
              </label>
              <input v-model="author_name" id="author_name" name="author_name" placeholder="Author Name" type="text" tabindex="2" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-grey" required>
            </div>
          </div>
          <div class="flex flex-wrap -mx-3 mb-2">
            <div class="w-full px-3">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-state">
                Description
              </label>
              <textarea v-model="description" name="description" id="description" tabindex="5" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-grey" rows="5"></textarea>
            </div>
          </div>
          <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full px-3">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-image">
                Image Path
              </label>
              <input v-model="image" id="image" name="image" placeholder="Image Path" type="text" tabindex="3" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-grey" required>
              <p class="text-grey-dark text-xs italic">Make it as long and as crazy as you'd like</p>
            </div>
          </div>
          <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full px-3">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-link">
                Web Link
              </label>
              <input v-model="link" id="link" name="link" placeholder="Image Path" type="text" tabindex="4" class="appearance-none block w-full bg-grey-lighter text-grey-darker border border-grey-lighter rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-grey" required>
              <p class="text-grey-dark text-xs italic">Make it as long and as crazy as you'd like</p>
            </div>
          </div>
          <div class="flex flex-wrap -mx-3 mb-2">
            <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-state">
                Category
              </label>
              <div class="relative">
                <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
                  <template slot-scope="{ result: { data, loading }, isLoading }">
                    <load-spin v-if="isLoading" :loading="isLoading"></load-spin>
                    <div v-else>
                        <select name="select_category" id="select_category" class="block appearance-none w-full bg-grey-lighter border border-grey-lighter text-grey-darker py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-grey">
                          <option :value="category.id"
                            v-for="category of data.categories"
                            :key="category.id"
                            class="link-margin">
                            {{ category.title }}
                          </option>
                        </select>
                    </div>
                  </template>
                </ApolloQuery>
                <div class="pointer-events-none absolute pin-y pin-r flex items-center px-2 text-grey-darker">
                  <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                </div>
              </div>
            </div>
            <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
              <label class="block uppercase tracking-wide text-grey-darker text-xs font-bold mb-2" for="grid-zip">
                Featured
              </label>
              <input class="mr-2 mt-3 leading-tight" type="checkbox">
              <span class="text-sm">
                Set Featured
              </span>
            </div>
            <div class="w-full md:w-1/2 px-3 py-5 mb-10 md:mb-0">
                <button type="submit" class="bg-blue hover:bg-blue-dark text-white font-bold py-2 px-4 rounded-full">
                    Submit
                </button>
            </div>
          </div>
        </form>
    </div>
  </div>
</template>

<script>
    import loadSpin from '@/components/Loader.vue'
    import addBook from '@/graphql/mutations/AddBook.gql'

    export default {
        components: {
            loadSpin
        },
        data () {
            return {
                title: '',
                author_name: '',
                image: '',
                link: '',
                description: '',
                featured: ''
            }
        },
        methods: {
            addBook() {
                const newTag = this.newTag
                this.newTag = ''
                // Call to the graphql mutation
                this.$apollo.mutate({
                  // Query
                  mutation: addBook,
                  // Parameters
                  variables: {
                    title: this.title,
                    author_name: this.author_name,
                    image: this.image,
                    link: this.link,
                    description: this.description,
                    featured: this.featured
                  },
                }).then((data) => {
                  console.log(data)
                  this.$router.push('/')
                }).catch((error) => {
                  console.error(error)
                  this.newTag = newTag
                })
            }
        }
    }
</script>

<style style="@/assets/main.css">
