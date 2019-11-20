<template>
  <div class="book">
    <h3>{{ name }}</h3>
    <p v-if="books.length == 0 || books === undefined">
      Looks like you haven't added any books yet...
    </p>
    <div v-else>
      Contains The Following Books:
      <ul>
        <li v-for="book in books" :key="`${name}-${book.id}`">
          {{ `${book.title} by ${book.author}` }}
          <button @click="sortBook(book)">⟰</button>
          <button @click="removeBook(book)">⟱</button>
          <button @click="removeBook(book)">X</button>
        </li>
      </ul>
      <h4>Sorting</h4>
      <button @click="sort(alphabetical)">Alphabetical Sort</button>
      <br />
      <br />
    </div>
    <!-- Edit state contains re-ordering -->
    <!-- <button @click="editing = !editing" v-if="!editing">Edit</button>
    <button @click="saveEdit(id)" v-if="editing">Save</button>
    <button @click="editing = !editing" v-if="editing">Cancel</button> -->
    <button @click="destroyCollection(id)">Destroy</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Collection",
  data: function() {
    return {
      editing: false
    };
  },
  props: {
    id: Number,
    name: String,
    books: Array
  },
  methods: {
    destroyCollection(collection) {
      // eslint-disable-next-line prettier/prettier
      axios.delete(`https://plushykingdom.com/api/collections/${collection}`).catch(function(error) {
        console.log(error);
      });
      this.$emit("delete-row");

      // TODO Force re-render or something to remove item
    },
    // saveCollection(collection) {
    //   axios.patch(`https://plushykingdom.com/api/collections/${collection.id}`, collection);
    //   this.$emit("editing", false);
    // },
    sortBook(book, direction) {
      axios.patch(`https://plushykingdom.com/api/collection/${book.id}`, {
        collection: this.id,
        sort: direction
      });
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
