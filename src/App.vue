<template>
  <!--using bootstrap class-->
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js CRUD with firebase </h1>
    </div>
  
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title"></input>
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author"></input>
          </div>
          <div class="form-group">
            <label for="bookUrl">URL:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url"></input>
          </div>
           <div class="form-group">
            <label for="bookPublishDate">Publish Date:</label>
            <datepicker :value="newBook.publishDate" class="form-control" ></datepicker>
            <!--input type="Datepicker" id="publishDate" class="form-control" v-model="newBook.publishDate"></input-->
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
  
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Title
              </th>
              <th>
                Author
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">
                  {{book.title}}</a>
              </td>
              <td>{{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'
import Firebase from 'firebase'
import Datepicker from 'vuejs-datepicker';
//access firebase
let config = {
  apiKey: "AIzaSyBuH5Q0wSMvW6IYoU92duMaewx-Wa3xv2I",
  authDomain: "vuejscrud.firebaseapp.com",
  databaseURL: "https://vuejscrud.firebaseio.com",
  projectId: "vuejscrud",
  storageBucket: "vuejscrud.appspot.com",
  messagingSenderId: "246355664837"
}

let app = Firebase.initializeApp(config);
let db = app.database();
//node of database in firebase
let booksRef = db.ref('books');

export default {
  name: 'app',
  components: {
    Datepicker
  },
  firebase: {
    //we are able to use database in firebase
    books: booksRef
  },

  data() {
    return {
      newBook: {
        key:'',
        title: '',
        author: '',
        url: '',
        publishDate: new Date()
      }
    }
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },

    removeBook:function(book){
      console.log(book['.key']);
      booksRef.child(book['.key']).remove();
    }
  }

}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
