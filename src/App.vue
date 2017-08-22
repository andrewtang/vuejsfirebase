<template>
  <div id="app" class="">
    <div class="ph5 ph5-ns">
      <div class="">
          <h2 class="f2">Book2 Store</h2>
        <hr>
        <form class="form-inline " v-on:submit.prevent="addBook">
          <label class="sr-only" for="bookTitle">Title</label>
          <input type="text" class="form-control pa1 ma2 mb-2 mr-sm-2 mb-sm-0" id="bookTitle" placeholder="Title" required v-model="newBook.title">
          <label class="sr-only" for="bookAuthor">Author</label>
          <input type="text" class="form-control pa1 ma2 mb-2 mr-sm-2 mb-sm-0" id="bookAuthor" placeholder="Author" required v-model="newBook.author">

          <label class="sr-only" for="bookUrl">Url</label>
          <input type="text" class="form-control pa1 ma2 mb-2 mr-sm-2 mb-sm-0" id="bookUrl" value="sadasdasdasd" required v-model="newBook.url">

          Book Cover Image URL<label class="sr-only" for="bookCover">Url</label>
          <input type="text" class="form-control pa1 ma2 mb-2 mr-sm-2 mb-sm-0" id="bookCover" value="sadasdasdasd" required v-model="newBook.cover">

          <button type="submit" class="btn btn-success">
            <i class="fa fa-plus" aria-hidden="true"></i> Add
          </button>

        </form>
        <hr>

      </div>
    </div>
    <br>
    <div class="ph5 ph5-ns">
      <h3 class="">Book Cards</h3>
      <br>
      <div class="w-100">
        <div v-for="book in books" class="b--dashed bg-lightest-blue dark-green fl ma2 w5 pa3 pa4-ns mv3 ba b--black-10">
          <div class="card text-center margin" style="">
            <div class="card-block">
              <img v-bind:src="book.cover" alt="Book Image Outline" class="w-100 db outline black-10"/>
              <h4 class="card-title">{{book.title}}</h4>
              <p class="card-text"> {{book.author}}</p>
              <p class="card-text"> {{book.description}}</p>
              <a v-bind:href="book.url" target="_blank"> <i class="fa fa-search" aria-hidden="true"></i> <span class="b "> Go to URL</span></a>
              <i class="fa fa-trash-o right" aria-hidden="true" v-on:click="removeBook(book)"></i>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import Hello from './components/Hello'
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyCvwPxoz6mC71_YIIRFlcTwkVsXGbO-w28",
  authDomain: "vuejs-firebase-book.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-book.firebaseio.com",
  projectId: "vuejs-firebase-book",
  storageBucket: "vuejs-firebase-book.appspot.com",
  messagingSenderId: "70958122285"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');


toastr.options = {
  "closeButton": false,
  "debug": false,
  "newestOnTop": true,
  "progressBar": false,
  "positionClass": "toast-top-right",
  "preventDuplicates": false,
  "onclick": null,
  "showDuration": "300",
  "hideDuration": "1000",
  "timeOut": "5000",
  "extendedTimeOut": "1000",
  "showEasing": "swing",
  "hideEasing": "linear",
  "showMethod": "fadeIn",
  "hideMethod": "fadeOut"
}

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://',
        cover: 'http://'
      }
    }
  },
  methods: {
    addBook: function(){
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = 'http://';
      this.newBook.cover = 'http://';
      toastr.success("Book added");
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.success("Book removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.card-heading h3{
  margin: 20px;
}

.card-block {

}
</style>
