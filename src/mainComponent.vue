<template>
  <div class="container-fluid mb-5">
    <!-- nav bar componente -->
    <navBarComponent
      @watchListEvent="displayWatchList"
      :numOfbooks="numberOfBooks"
      :price="TotalPrice"
    />

    <!-- books component  -->
    <keep-alive>
      <booksComponenet
        @bookAddedEvent="handleNavBarEvent"
        :bookListUpdated="bookList"
        v-if="isWatchListVisible == false"
      />
    </keep-alive>

    <!-- watch list component  -->
    <keep-alive>
      <watchListComponent
        @removeFromWatchList="updateWatchList"
        :numOfbooks="numberOfBooks"
        :books="bookList"
        :price="TotalPrice"
        v-if="isWatchListVisible == true"
      />
    </keep-alive>
  </div>
</template>

<script>
import navBarComponent from "./components/navBarComponent.vue";
import booksComponenet from "./components/booksComponente.vue";
import watchListComponent from "./components/watchListComponent.vue";

export default {
  data: () => ({
    isWatchListVisible: false,
    bookList: [],
    numberOfBooks: 0,
    TotalPrice: 0.0,
  }),

  methods: {
    handleNavBarEvent(numOfBooks, price, books) {
      (this.numberOfBooks = numOfBooks),
        (this.TotalPrice = price),
        (this.bookList = books);
    },

    displayWatchList(status) {
      this.isWatchListVisible = status;
    },

    updateWatchList(bookList) {
      this.bookList = bookList;

      console.log(this.bookList.length);
    },
  },

  computed: {},
  components: {
    booksComponenet,
    navBarComponent,
    watchListComponent,
  },
};
</script>