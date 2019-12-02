<template>
  <div class="book">
    <p v-if="books.length == 0 || books === undefined">
      Looks like you haven't added any books yet...
    </p>
    <div v-else>
      Contains The Following Books:
      <ul>
        <!-- <li v-for="book in books" :key="`${name}-${book.id}`"> -->
        <li v-for="book in books" :key="`$${book.id}`">
          <!-- {{ `${book.title} by ${book.author}` }} -->
          {{ book['book_id'] }}
          <button @click="sortBook(book['book_id'], book['book_id'] + 1)">⟰</button>
          <button @click="sortBook(book['book_id'], book['book_id'] - 1)">⟱</button>
          <button @click="removeBook(book['book_id'])">X</button>
        </li>
      </ul>
      <h4>Sorting</h4>
      <button @click="sort(alphabetical)">Randomize</button>
      <br />
      <br />
    </div>
    <!-- Edit state contains re-ordering -->
    <!-- <button @click="editing = !editing" v-if="!editing">Edit</button>
    <button @click="saveEdit(id)" v-if="editing">Save</button> -->
    <button @click="editing = !editing" v-if="editing">Cancel</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Collection",
  data: function() {
    return {
      order: [],
      editing: false
    };
  },
  props: {
    books: Array
  },
  methods: {
    sortBook(origin, destination) {
      axios.post(`https://plushykingdom.com/api/collection/sort`, {
        type: "swap",
        originId: origin,
        destinationId: destination
      });
    },
    removeBook(book) {
      axios.delete(`https://plushykingdom.com/api/collection/books/${book}`);
      this.$emit("delete-book", book);
    }
  }
};
</script>

<style lang="scss" scoped>
.book {
  ul {
    text-align: left;
    width: 100%;
  }

  button:hover {
    background: #7ed1ab;
  }
}

@media only screen and (max-width: 1100px) {
  .book {
    border: 2px solid #7ed1ab;
    border-radius: 12px;
  }
}
</style>
