<template>
  <div>
    <input type="text" class="book-input" placeholder="New Book Name" v-model="newBook" @keyup.enter="addBook">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <book-item v-for="book in booksFiltered" :key="book.id" class="book-item" :book="book" :index="book" :checkAll="anyAvailable" @removedBook="removeBook" @finishedEdit="finishedEdit">
      </book-item>
    </transition-group>

    <div class="extra-container">
      <div>
        <label><input type="checkbox" @change="checkAllBooks()" :checked="anyAvailable">Check All</label>
      </div>
      <div>{{available}} available book/s</div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'available' }" @click="filter = 'available'">Available</button>
        <button :class="{ active: filter == 'notavailable' }" @click="filter = 'notavailable'">Not Available</button>
      </div>
      <div>
        <transition name="fade">
         <button v-if="showClearUnavailableButton" @click="clearUnavailable">Remove Unavailable</button>
        </transition>
      </div>

    </div>
  </div>
</template>

<script>
import BookItem from './BookItem'

export default {
  name: 'book-list',
  components: {
    BookItem
  },
  data () {
    return {
      newBook: '',
      idForBook: 3,
      beforeEditCache: '',
      filter: 'all',
      books: [
        {
          'id':1,
          'title':'Jerwin Adventures',
          'status': false,
          'editing': false,
        },
        {
          'id':2,
          'title':'Sword Art Online',
          'status': false,
          'editing': false,
        },
      ]
    }
  },
  computed: {
    available() {
      return this.books.filter(book => book.status).length
    },
    anyAvailable() {
        return this.available == this.books.length;
    },
    booksFiltered() {
      if (this.filter == 'all') {
        return this.books
      } else if (this.filter == 'available') {
        return this.books.filter(book => book.status)
      } else if (this.filter == 'notavailable') {
        return this.books.filter(book => !book.status)
      }
      return this.books
    },
    showClearUnavailableButton() {
      if (this.books.filter(book => !book.status).length > 0) {
        return true 
      }
    }
  },
  methods: {
    addBook() {
      if (this.newBook.trim().length == 0) {
        return
      }
      this.books.push({
        id: this.idForBook,
        title: this.newBook,
        status: true,
        editing: false,
      })

      this.newBook = ''
      this.idForBook++
    },
    removeBook(id) {
      const index = this.books.findIndex((item) => item.id == id)
      this.books.splice(index,1)
    },
    checkAllBooks() {
      this.books.forEach((book) => book.status = event.target.checked)
    },
    clearUnavailable() {
      this.books = this.books.filter(book => book.status)
    },
    finishedEdit(data) {
      const index = this.books.findIndex((item) => item.id == data.id)
      this.books.splice(index, 1, data)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

  .book-input {
    width: 100%;
    padding:10px 18px;
    font-size: 10px;
    margin-bottom: 16px;

    &:focus {
      outline:0;
    }
  }

  .book-item {
    margin-bottom: 12px;
    display:flex;
    align-items: center;
    justify-content: space-between;

    animation-duration: 0.3s;
  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;

    &:hover {
      color:black;
    }
  }

  .book-item-left {
    display: flex;
    align-items: center;
  }

  .book-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }

  .book-item-edit {
    color:#2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: 'Avenir',Helvetica, Arial, sans-serif;

    &:focus {
      outline: none;
    }
  }

  .available {
    background-color: green;
    padding: 5px;
    color: white;
    border-radius: 10px;
  }
  .not-available {
    background-color: red;
    padding: 5px;
    color: white;
    border-radius: 10px;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }
  
  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    &:hover {
      background: lightgreen;
    }
    &:focus {
      outline: none;
    }
  }

  .active {
    background: lightgreen;
  }

  // CSS Transitions
  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>
