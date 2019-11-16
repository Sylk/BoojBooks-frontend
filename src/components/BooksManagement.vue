<template>
  <div>
    <h3 class="management-header">Your current Collections:</h3>
    <h3 class="management-header">The Books Available Currently:</h3>
    <section class="book-shelf">
      <div v-for="book in books" :key="book.id" class="book-binding">
        <book
          :id="book.id"
          :title="book.title"
          :author="book.author"
          :edition="book.edition"
          :length="book.length"
          :score="book.score"
          :cover="book.cover"
          :file="book.file"
          :publishedDate="book.publishedDate"
          :editing="book.editing"
        />
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import Book from "@/components/Book.vue";

export default {
  name: "BooksManagement",
  components: {
    Book
  },
  mounted() {
    this.getBooks();
  },
  data() {
    return {
      books: [],
    };
  },
  methods: {
    async getBooks() {
      try {
        const response = await axios.get("http://boojbooks.test/api/books");
        this.books = response.data.data;

        this.books.forEach(book => {
          book.editing = false;
        });

        console.log(this.books);
      } catch (error) {
        // TODO: feed this to a debugging application such as sentry, telescope, etc...
        // console.log({ error });
      }
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.management-header {
  margin-bottom: 15px;
}

.book-shelf {
  display: flex;
  flex-flow: row wrap;
  justify-content: flex-start;
}

.book-binding {
  width: 25%;
  margin-bottom: 25px;
}

@media only screen and (max-width: 1100px) {
  .book-binding {
    width: 100%;
    padding-left: 10px;
    padding-right: 10px;
    margin-bottom: 25px;
  }
}
</style>
