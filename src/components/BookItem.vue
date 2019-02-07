<template>
  <div class="book-item">
    <div class="book-item-left">
      <input type="checkbox" v-model="status" @change="doneEdit">
      <div v-if="!editing" @dblclick="editBook" class="book-item-label">{{title}}</div>
      <input v-else class="book-item-edit" type="text" v-model="title" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus>
      <span v-if="status" class="available">Available</span>
      <span v-else class="not-available">Not Available</span>
    </div>
    <div class="remove-item" @click="removeBook(book.id)">
      &times;
    </div>
  </div>
</template>

<script>
  export default {
    name: 'book-item',
    props: {
      book: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
        required: true
      },
      checkAll: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {
        'id': this.book.id,
        'title': this.book.title,
        'status': this.book.status,
        'editing': this.book.editing,
        'beforeEditCache':'',
      }
    },
    watch: {
      checkAll: function() {
        this.status = this.checkAll ? true : this.book.status
      }
    },
    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    },
    methods: {
      removeBook: function(id) {
        this.$emit('removedBook',id)
      },
      editBook: function() {
        this.beforeEditCache = this.title;
        this.editing = true;
      },
      doneEdit: function() {
        if (this.title.trim() == '') {
          this.title = this.beforeEditCache
        }
        this.editing = false
        this.$emit('finishedEdit',{
          'id': this.id,
          'title': this.title,
          'status': this.status,
          'editing': this.editing,
        })
      },
      cancelEdit: function() {
        this.title = this.beforeEditCache
        this.editing = false
      },
    }
  }
</script>