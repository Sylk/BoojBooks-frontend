<template>
  <div>
    <h3 class="management-header">Your current Collections:</h3>
    <h3 class="management-header">The Books Available Currently:</h3>
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
          :collections="collections"
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
      collections: ["Swag", "Money", "420blazeit"],
      createdBook: { creating: false, title: null, author: null },
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
        console.log({ error });
      }
    },

        console.log(this.collections);
      } catch (error) {
        // TODO: feed this to a debugging application such as sentry, telescope, etc...
        // console.log({ error });
      }
    },
    createBook(title, author) {
      console.log(title, author);
      axios
        .post("http://boojbooks.test/api/books", {
          title: title,
          author: author
        })
        .catch(function(error) {
          console.log(error);
        });

      this.createdBook = { creating: false, title: null, author: null };
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
