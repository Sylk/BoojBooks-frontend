<template>
  <div class="book">
    <img :src="cover" width="85%" />
    <h3 class="title">{{ title }}</h3>
    <h4 class="author">by {{ author }}</h4>
    Book Metrics:
    <ul>
      <li>Edition: {{ edition }}</li>
      <li>Length: {{ length }}</li>
      <li>Score: {{ score }} out of 100</li>
      <li>Published: {{ publishedDate }}</li>
    </ul>
    <h4 style="margin-bottom:2px;">
      Add To Collection?<button style="margin-left:6px;" @click="addToCollection(id)">Add</button>
    </h4>
    <br />
    <button @click="openBook(file)">Read</button>
    <button @click="editing = !editing" v-if="!editing">Edit</button>
    <button @click="saveEdit(id)" v-if="editing">Save</button>
    <button @click="editing = !editing" v-if="editing">Cancel</button>
    <button @click="destroyBook(id)" :class="{ disabled: !editing }">Destroy</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Book",
  data: function() {
    return {
      editing: false,
      newCollection: null
    };
  },
  props: {
    id: Number,
    title: String,
    author: String,
    edition: Number,
    length: Number,
    score: Number,
    cover: String,
    file: String,
    publishedDate: String
  },
  methods: {
    openBook(file) {
      window.open(file);
      window.focus();
    },
    destroyBook(bookId) {
      axios.delete(`https://plushykingdom.com/api/books/${bookId}`);
      this.$emit("delete-row");
    },
    saveBook(book) {
      axios.patch(`https://plushykingdom.com/api/books/${book.id}`, book);
      this.$emit("editing", false);
    },
    addToCollection(bookId) {
      axios
        .post("https://plushykingdom.com/api/collection/books", {
          bookId: bookId
        })
        .then(response => {
          this.$emit("add-to-collection", response.data);
        });
      // emit created event with the book id
    }
  }
};
</script>

<style lang="scss" scoped>
.book {
  .title {
    margin-top: -80px;
    color: #ffffff;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
  }
  .author {
    color: #ffffff;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
  }
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
    max-height: 517px;
    padding-bottom: 10px;
    padding-top: 15px;
  }
}
</style>
