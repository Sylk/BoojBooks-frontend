<template>
  <div>
    <h1 class="management-header">Your current Collection:</h1>
    <section class="book-shelf">
      <div class="collection-binding">
        <collection :books="collection" v-on:delete-book="deleteBookFromCollection()" />
      </div>
    </section>
    <h1 class="management-header">The Books Available Currently:</h1>
    <section class="book-shelf">
      <div class="book-binding new-book">
        <button
          class="book-start-creation"
          v-if="!createdBook.creating"
          @click="createdBook.creating = !createdBook.creating"
        >
          Create New Book
        </button>
        <div class="book-creation-form" v-if="createdBook.creating">
          <input type="text" placeholder="Title" v-model="createdBook.title" />
          <input type="text" placeholder="Author" v-model="createdBook.author" />
          <input type="text" placeholder="Edition" v-model="createdBook.edition" disabled />
          <input type="text" placeholder="Length" v-model="createdBook.length" disabled />
          <input type="text" placeholder="Score" v-model="createdBook.score" disabled />
          <input type="text" placeholder="Cover" v-model="createdBook.cover" disabled />
          <input type="text" placeholder="File" v-model="createdBook.file" disabled />
          <input
            type="text"
            placeholder="Published Date"
            v-model="createdBook.publishedDate"
            disabled
          />
          <button
            class="book-finish-creation"
            @click="createBook(createdBook.title, createdBook.author)"
          >
            Create
          </button>
          <button
            class="book-finish-creation"
            @click="createdBook.creating = !createdBook.creating"
          >
            Cancel
          </button>
        </div>
      </div>
      <div v-for="(book, index) in books" :key="book.id" class="book-binding">
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
          v-on:delete-row="deleteBook(index)"
          v-on:add-to-collection="addToCollection(book)"
        />
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import Book from "@/components/Book.vue";
import Collection from "@/components/Collection.vue";

export default {
  name: "BooksManagement",
  components: {
    Book,
    Collection
  },
  mounted() {
    this.getCollections();
    this.getBooks();
  },
  data() {
    return {
      books: [],
      collection: [],
      createdBook: { creating: false, title: null, author: null },
      createdCollection: { creating: false, collection: null }
    };
  },
  methods: {
    async getBooks() {
      const response = await axios.get("https://plushykingdom.com/api/books");
      this.books = response.data.data;

      this.books.forEach(book => {
        book.editing = false;
      });
    },
    async getCollections() {
      const response = await axios.get("https://plushykingdom.com/api/collections");
      this.collection = response.data.data;
    },
    createBook(title, author) {
      axios
        .post("https://plushykingdom.com/api/books", {
          title: title,
          author: author
        })
        .then(response => {
          this.addBook(response);
        });
      this.createdBook = { creating: false, title: null, author: null };
    },
    addToCollection: function(book) {
      book = { book_id: book.id, created_at: book.publishedDate };
      this.collection.push(book);
    },
    deleteBookFromCollection: function(book) {
      this.collection.splice(book, 1);
    },
    addBook: function(book) {
      // Note: It understands it was made, but all of the values are undefined...
      this.books.push(book);
    },
    deleteBook: function(index) {
      this.books.splice(index, 1);
    }
  }
};
</script>

<style lang="scss" scoped>
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

.collection-binding {
  width: 50%;
  margin-bottom: 25px;
}

.new-book {
  display: flex;
  align-items: flex-start;
  justify-content: center;

  .book-start-creation {
    padding: 60px 40px;
    font-size: 14px;
    margin-top: 47px;
  }

  .book-creation-form {
    margin-top: 97px;
  }

  .book-finish-creation {
    margin-top: 14px;
  }
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
