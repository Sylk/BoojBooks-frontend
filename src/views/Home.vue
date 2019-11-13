<template>
  <div class="home">
    <p>
      Welcome to BoojBooks, this is the frontend application portion of the test.
      <br>
      Documentation for the requirements is available <a href="https://github.com/ActiveWebsite/boojbooks">here</a>
    </p>
    <BooksManagement/>
  </div>
</template>

<script>
// @ is an alias to /src
import BooksManagement from '@/components/BooksManagement.vue';

export default {
  name: 'home',
  components: {
    BooksManagement,
  },
};
</script>
