<template>
  <div>
    <h1 class="management-header">Your current Collections:</h1>
    <section class="book-shelf">
      <div class="collection-binding new-book">
        <button
          class="book-start-creation"
          v-if="!createdCollection.creating"
          @click="createdCollection.creating = !createdCollection.creating"
        >
          Create New Collection
        </button>
        <div class="book-creation-form" v-if="createdCollection.creating">
          <input
            type="text"
            placeholder="Collection"
            v-model="createdCollection.collection"
            style="width: 100%;"
          />
          <button
            class="book-finish-creation"
            @click="createCollection(createdCollection.collection)"
          >
            Create
          </button>
          <button
            class="book-finish-creation"
            @click="createdCollection.creating = !createdCollection.creating"
          >
            Cancel
          </button>
        </div>
      </div>
      <div
        v-for="(collection, index) in collections"
        :key="collection.id"
        class="collection-binding"
      >
        <collection
          :id="collection.id"
          :name="collection.name"
          :books="collection.books"
          v-on:delete-row="deleteCollection(index)"
        />
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
          :collections="collections"
          v-on:delete-row="deleteBook(index)"
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
      collections: [],
      createdBook: { creating: false, title: null, author: null },
      createdCollection: { creating: false, collection: null }
    };
  },
  methods: {
    async getBooks() {
      try {
        const response = await axios.get("https://plushykingdom.com/api/books");
        this.books = response.data.data;

        this.books.forEach(book => {
          book.editing = false;
        });
        console.log("Books");
        console.log(this.books);
      } catch (error) {
        // TODO: feed this to a debugging application such as sentry, telescope, etc...
        console.log({ error });
      }
    },
    async getCollections() {
      try {
        const response = await axios.get("https://plushykingdom.com/api/collections");
        this.collections = response.data.data;
        console.log("Collections");
        console.log(response);
      } catch (error) {
        // TODO: feed this to a debugging application such as sentry, telescope, etc...
        // console.log({ error });
      }
    },
    createBook(title, author) {
      console.log(title, author);
      axios
        .post("https://plushykingdom.com/api/books", {
          title: title,
          author: author
        })
        .then(response => {
          console.log("Book Response");
          console.log(response);
          this.addBook(response);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.createdBook = { creating: false, title: null, author: null };
    },
    createCollection(collection) {
      console.log(collection);
      axios
        .post("https://plushykingdom.com/api/collections", {
          collection: collection
        })
        .then(response => {
          console.log("Collection");
          console.log(response);
          this.addCollection(response);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.createdCollection = { creating: false, collection: null };
    },
    addCollection: function(collection) {
      console.log(collection.id);
      collection = [
        {
          id: collection.id,
          name: collection.name,
          books: []
        }
      ];

      // collection.push({ books: [] });
      this.collections.push(collection);
      console.log(this.collections);
    },
    deleteCollection: function(index) {
      this.collections.splice(index, 1);
    },
    addBook: function(book) {
      console.log(book);
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
