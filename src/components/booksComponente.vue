<template>
  <div class="container-fluid mb-5">
    <div class="container-fluid mt-3">
      <div class="row d-flex justify-content-around text-center">
        <div
          v-for="book in books"
          :key="book.id"
          :class="[
            'card p-0 mb-3 border border-1',
            book.pages < 50 ? 'border-danger' : 'border-success',
          ]"
          style="width: 20rem; margin: 0.4rem"
        >
          <img
            :src="book.image"
            style="max-height: 300px"
            class="card-img-top"
          />

          <div
            class="
              card-footer
              fs-4
              font-weight-bold
              border border-bottom-2
              text-center
            "
          >
            {{ book.name }}
          </div>

          <div class="card-body py-0">
            <div class="book-content">
              <div class="row d-flex justify-content-around">
                <div class="col-12 my-2">
                  <span>Category </span> :
                  {{ book.category }}
                </div>

                <div class="col-12">
                  <span>Author </span> :
                  {{ book.author }}
                </div>

                <div class="col-6 my-2">
                  <span>Pages </span> :
                  <span :class="book.pages < 50 ? 'less' : 'more'">{{
                    book.pages
                  }}</span>
                </div>

                <div class="col-6 my-2">
                  <span>Price </span> :
                  {{ handleCurrency(book.price.value) }}
                </div>

                <div class="col-12 mb-2">
                  <span>ISBN </span> :
                  {{ book.ISBN }}
                </div>
              </div>
            </div>
          </div>

          <div class="card-footer">
            <button
              class="btn btn-success"
              :disabled="checkExist(book.id)"
              @click="addToList(book)"
            >
              Add to List
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
      books: [],
      booksList: [],
      isWatchListVisible: false,
    };
  },

  async created() {
    // fetch data from db server
    const response = await fetch("http://localhost:5000/books");
    this.books = await response.json();
  },

  updated(){
    this.updateNavBar();
  },

  

  methods: {
    addToList(book) {
      this.booksList.push({
        book: book,
      });

      let totalPrice = this.handleCurrency(this.getTotalPrice());
      let numOfBooks = this.getbooksListCount();
      let bookList = this.booksList;

      this.$emit("bookAddedEvent", numOfBooks, totalPrice, bookList);
    },

    getTotalPrice() {
      let total = 0;
      for (let item of this.booksList) {
        total += item.book.price.value;
      }
      return total;
    },

    getbooksListCount() {
      return this.booksList.length;
    },

    handleCurrency(value) {
      // [ en-US --> USD,  ar-EG --> EGP, ar-SA --> SAR]
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(value);
    },

    checkExist(bookID) {
      return this.booksList.some((item) => item.book.id == bookID);
    },

    updateNavBar () {
        this.booksList = this.$props.bookListUpdated;
        
        let totalPrice = this.handleCurrency(this.getTotalPrice());
        let numOfBooks = this.getbooksListCount();
        let bookList = this.booksList;

        this.$emit("bookAddedEvent", numOfBooks, totalPrice, bookList);
    
    },

  },

  computed: {},
  components: {},

  props: ["bookListUpdated"],
};

</script>

<style scoped>
.less {
  color: red;
}

.more {
  color: green;
}

/* card */
.card-body span {
  font-weight: bolder;
}
</style>