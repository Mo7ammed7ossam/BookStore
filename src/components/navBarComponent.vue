<template>
  <nav class="navbar navbar-dark bg-dark align-items-baseline">
    <div class="container-fluid">
      <a
        href="#"
        @click.prevent="displayWatchList('hide')"
        class="navbar-brand fs-3"
        >Books</a
      >

      <div class="d-flex align-items-baseline text-light">
        <p>
          [ {{ numOfbooks }} ] {{ numOfbooks == 1 ? "Book" : "Books" }} wiht
          total price : [ {{ price }} ]
        </p>

        <button class="btn btn-success mx-2" @click="displayWatchList('show')">
          Show List
        </button>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
    };
  },

  methods: {
    displayWatchList(status) {
      var isWatchListVisible;
      if (status == "hide") {
        isWatchListVisible = false;
      } else {
        isWatchListVisible = true;
      }
    this.$emit('watchListEvent', isWatchListVisible);
    },

  },

  props: ["numOfbooks", "price"],

  
};
</script>

<style>
</style>