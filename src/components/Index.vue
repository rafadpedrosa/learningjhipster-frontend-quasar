<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <q-toolbar-title :padding="0">
        Quasar Framework v{{$q.version}}
      </q-toolbar-title>
    </div>

    <div class="layout-view">
      <br/>
      <br/>
      <center>
        <h5>Create Book</h5>
        <input v-model="book.name" placeholder="Book name">
        <button class="primary" @click="submit">
          Submit
        </button>
        <button class="primary" @click="reloadlist">
          Reload
        </button>
        <hr>
        <h5>Find Book by id and Edit</h5>
        <input v-model="bookEdit.id" placeholder="Book id"><br>
        <input v-model="bookEdit.name" placeholder="Book name">
        <br>
        <button class="primary" @click="findById(bookEdit.id)">
          Find by id
        </button>
        <button class="primary" @click="edit">
          Edit book
        </button>
        <br>
        <br>
        {{ bookFetched }}
        <hr>
        <h5>Books List</h5>
        <div v-for="book in books">
          <a href="#" @click="findById(book.id)">{{ book.id }} | {{ book.name }}</a>
        </div>
      </center>
    </div>
  </q-layout>
</template>

<script>
  import { Toast } from 'quasar'

  export default {
    data() {
      return {
        book: {
          'name': 'teste default'
        },
        bookEdit: {
          'id':"",
          'name': ''
        },
        bookFetched: {
          'id':"",
          'name': ''
        },
        books: []
      }
    },
    methods: {
      submit() {
        if (this.book.name != "") {
          this.$http.post('http://localhost:8081/api/books', this.book).then(response => {
            Toast.create('Book created!');
            this.reloadlist();
          }, error => {
            Toast.create('An erro happens during the inserting!');
          })
          this.book.name = "";
        }else{
          Toast.create('The book can\'t is empty');
        }
      },
      reloadlist (){
        this.$http.get('http://localhost:8081/api/books').then(response => {
          this.books = response.body;
          Toast.create('Books fetched!');
        }, error => {
          Toast.create('An erro happens during the fetch of books!');
          // console.log(error)
        })
      },
      findById(id){
        id = id == undefined ? this.bookEdit.id : id;
        if (id != "") {
          this.$http.get('http://localhost:8081/api/books/'+id).then(response => {
            // console.log(response.body);
            this.bookFetched = response.body;
            this.bookEdit.id = this.bookFetched.id;
            this.bookEdit.name = this.bookFetched.name;
            Toast.create('Book finded!');
          }, error => {
            Toast.create('An erro happens during finding book!');
            // console.log(error)
            this.bookFetched = {'id':"0",'name': 'Not Find'};
          })
        }else{
          Toast.create('Book id can\'t be empty!');
        }
      },
      edit(){
        this.$http.put('http://localhost:8081/api/books/',this.bookEdit).then(response => {
          // console.log(response.body);
          Toast.create('Book Edited!');
          this.reloadlist();
        }, error => {
          Toast.create('An erro happens during editing book!');
          // console.log(error)
        })
        this.bookEdit = {'id':"",'name': ''};
      },
    },
    created () {
      this.reloadlist();
    }
  }
</script>

<style lang="stylus">
</style>
