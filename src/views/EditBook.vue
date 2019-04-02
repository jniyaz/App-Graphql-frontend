<template>
  <div class="about">
    <div>
        <form id="contact" action="" method="post" @submit.prevent="editBook">
        <h3>Edit Book</h3> <br>
        <fieldset>
            <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
              <!-- The result will automatically updated -->
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <!-- Some content -->
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                    <label for="category">Choose Category : </label>
                    <select name="select_category" id="select_category" class="mb20">
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
        </fieldset>
        <fieldset>
          <input v-model="title" id="book_name" name="book_name" placeholder="Book Name" type="text" tabindex="1" required autofocus>
        </fieldset>
        <fieldset>
          <input v-model="author_name" id="author_name" name="author_name" placeholder="Author Name" type="text" tabindex="2" required>
        </fieldset>
        <fieldset>
          <input v-model="image" id="image" name="image" placeholder="Image Path" type="text" tabindex="3" required>
        </fieldset>
        <fieldset>
          <input v-model="link" id="link" name="link" placeholder="Website Link" type="url" tabindex="4" required>
        </fieldset>
        <fieldset>
          <textarea v-model="description" id="description" name="description" placeholder="Description" tabindex="5" required></textarea>
        </fieldset>
        <fieldset>
            <input v-model="featured" type="checkbox" id="featured" name="featured"> Featured
        </fieldset>
        <fieldset>
          <button name="submit" type="submit" id="contact-submit" data-submit="...saving">Save</button>
        </fieldset>
      </form>
    </div>
  </div>
</template>

<script>
    import updateBook from '@/graphql/mutations/updateBook.gql'

    export default {
        data () {
            return {
                id: '',
                title: '',
                author_name: '',
                image: '',
                link: '',
                description: '',
                featured: ''
            }
        },
        methods: {
            editBook() {
                const newTag = this.newTag
                this.newTag = ''
                // Call to the graphql mutation
                this.$apollo.mutate({
                  // Query
                  mutation: updateBook,
                  // Parameters
                  variables: {
                    id: this.id,
                    title: this.title,
                    author_name: this.author_name,
                    image: this.image,
                    link: this.link,
                    description: this.description,
                    featured: this.featured
                  },
                }).then((data) => {
                  console.log(data)
                  this.$router.push('/books/' + this.id)
                }).catch((error) => {
                  console.error(error)
                  this.newTag = newTag
                })
            }
        }
    }
</script>

<style>
    @import url(https://fonts.googleapis.com/css?family=Open+Sans:400italic,400,300,600);

    * {
        margin:0;
        padding:0;
        box-sizing:border-box;
        -webkit-box-sizing:border-box;
        -moz-box-sizing:border-box;
        -webkit-font-smoothing:antialiased;
        -moz-font-smoothing:antialiased;
        -o-font-smoothing:antialiased;
        font-smoothing:antialiased;
        text-rendering:optimizeLegibility;
    }

    .container {
        max-width:400px;
        width:100%;
        margin:0 auto;
        position:relative;
    }

    #contact input[type="text"], #contact input[type="email"], #contact input[type="tel"], #contact input[type="url"], #contact textarea, #contact button[type="submit"] { font:400 12px/16px "Open Sans", Helvetica, Arial, sans-serif; }

    #contact {
        background:#F9F9F9;
        padding:25px;
        margin:50px 0;
    }

    #contact h3 {
        color: #F96;
        display: block;
        font-size: 30px;
        font-weight: 400;
    }

    #contact h4 {
        margin:5px 0 15px;
        display:block;
        font-size:13px;
    }

    fieldset {
        border: medium none !important;
        margin: 0 0 10px;
        min-width: 100%;
        padding: 0;
        width: 100%;
    }

    #contact input[type="text"], #contact input[type="email"], #contact input[type="tel"], #contact input[type="url"], #contact textarea {
        width:100%;
        border:1px solid #CCC;
        background:#FFF;
        margin:0 0 5px;
        padding:10px;
    }

    #contact input[type="text"]:hover, #contact input[type="email"]:hover, #contact input[type="tel"]:hover, #contact input[type="url"]:hover, #contact textarea:hover {
        -webkit-transition:border-color 0.3s ease-in-out;
        -moz-transition:border-color 0.3s ease-in-out;
        transition:border-color 0.3s ease-in-out;
        border:1px solid #AAA;
    }

    #contact textarea {
        height:100px;
        max-width:100%;
      resize:none;
    }

    #contact button[type="submit"] {
        cursor:pointer;
        width:100%;
        border:none;
        background:#0CF;
        color:#FFF;
        margin:0 0 5px;
        padding:10px;
        font-size:15px;
    }

    #contact button[type="submit"]:hover {
        background:#09C;
        -webkit-transition:background 0.3s ease-in-out;
        -moz-transition:background 0.3s ease-in-out;
        transition:background-color 0.3s ease-in-out;
    }

    #contact button[type="submit"]:active { box-shadow:inset 0 1px 3px rgba(0, 0, 0, 0.5); }

    #contact input:focus, #contact textarea:focus {
        outline:0;
        border:1px solid #999;
    }
    ::-webkit-input-placeholder {
     color:#888;
    }
    :-moz-placeholder {
     color:#888;
    }
    ::-moz-placeholder {
     color:#888;
    }
    :-ms-input-placeholder {
     color:#888;
    }

</style>