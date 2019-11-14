<template>
  <div>
    <h3>The Books Available Currently:</h3>
    <div v-for="book in books" :key="book.id">
      <book
        :title="book.title"
        :author="book.author"
        :edition="book.edition"
        :length="book.length"
        :score="book.score"
        :cover="book.cover"
        :file="book.file"
        :releaseDate="book.releaseDate"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Book from '@/components/Book.vue';

export default {
  name: 'BooksManagement',
  components: {
    Book,
  },
  beforeMount() {
    this.getBooks();
  },
  data() {
    return {
      books: [],
    };
  },
  methods: {
    getBooks() {
      try {
        const response = axios.get('//plushykingdom.com/api/books');
        this.books = response;
        console.log('Books are as follow');
        console.log(this.books);
      } catch (error) {
        // TODO: feed this to a debugging application such as sentry, telescope, etc... 
        console.log({ error });
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
</style>
