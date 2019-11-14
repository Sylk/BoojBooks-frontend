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
import Book from '@/components/Book.vue';

export default {
  name: 'BooksManagement',
  components: {
    Book,
  },
  data() {
    return {
      books: [
        {
          id: 0,
          title: 'Gone',
          author: 'Henry Zam',
          edition: 1,
          length: 262,
          score: 10,
          file: '@/assets/ram.jpg',
          releaseDate: '2006-1-7T03:02:00',
        },
      ],
    };
  },
  methods: {
    getBooks() {},
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
