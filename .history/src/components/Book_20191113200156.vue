<template>
  <div class="book">
    <img :src="'@/assets/' + cover" width="100%" height=""/>
    {{cover}}
    <h3>{{title}} by {{author}}</h3>
    Book Metrics:
    <ul>
      <li>Edition: {{edition}}</li>
      <li>Length: {{length}}</li>
      <li>Score: {{score}} out of 10</li>
      <li>PDF Location: <a :href="'/books/' + file"> {{file}}</a></li>
      <li>Published: {{releaseDate}}</li>
    </ul>
    <button>Edit</button>
    <button>Destroy</button>
  </div>
</template>

<script>
export default {
  name: 'Book',
  props: {
    title: String,
    author: String,
    edition: Number,
    length: Number,
    score: Number,
    cover: String,
    file: String,
    releaseDate: String,
  },
};
</script>

<style lang="scss" scoped>
.book {
  width: 25%;
  height: 200px;

  ul {
    text-align: left;
  }
}
</style>
