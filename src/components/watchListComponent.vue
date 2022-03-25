<template>
  <div class="container mt-3">
    <div class="alert alert-danger" role="alert" v-if="numOfbooks == 0">
      <h4 class="alert-heading">Watch list Empty</h4>
      Sorry Your Watch list Empty , please add some Books !
    </div>

    <div class="watchList" v-if="numOfbooks > 0">
      <div class="table-responsive">
        <table
          class="table table-hover table-bordered table-striped text-center"
        >
          <thead>
            <tr>
              <th>ID</th>
              <th>Book Name</th>
              <th>Category</th>
              <th>Author</th>
              <th>Pages</th>
              <th>Price</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in booksList" :key="item.book.id">
              <td>{{ item.book.id }}</td>
              <td>{{ item.book.name }}</td>
              <td>{{ item.book.category }}</td>
              <td>{{ item.book.author }}</td>
              <td>{{ item.book.pages }}</td>
              <td>{{ handleCurrency(item.book.price.value) }}</td>
              <td>
                <button
                  class="btn btn-danger"
                  type="button"
                  @click="removeFromList(item)"
                >
                  Remove
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <th colspan="3">Total Price:</th>
              <td colspan="3">{{ price }}</td>
              <td>
                <button class="btn btn-primary" @click.prevent="checkout">
                  checkout
                </button>
              </td>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      booksList: this.$props.books,
    };
  },

  methods: {
    handleCurrency(value) {
      // [ en-US --> USD,  ar-EG --> EGP, ar-SA --> SAR]
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(value);
    },

    removeFromList(book) {
      this.booksList.splice(
        this.booksList.findIndex((item) => item.book.id == book.book.id),
        1
      );

      let bookList = this.booksList;
      this.$emit("removeFromWatchList", bookList);
    },

    checkout() {
      let conf = window.confirm("Are you sure you want to checkout?");

      this.booksList.forEach(book => {
        this.booksList.splice(book)
      });

      let bookList = this.booksList;
      this.$emit("removeFromWatchList", bookList);
    },
  },

  props: ["numOfbooks", "price", "books"],
};
</script>

<style>
</style>