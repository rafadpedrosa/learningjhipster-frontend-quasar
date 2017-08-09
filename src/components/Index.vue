<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <q-toolbar-title :padding="0">
        Quasar Framework v{{$q.version}}
      </q-toolbar-title>
    </div>
  
    <!--
            Replace following "div" with
            "<router-view class="layout-view">" component
            if using subRoutes
          -->
    <div class="layout-view">
      <br/>
      <br/>
      <center>
        <div v-for="book in books">
          {{book.name}}
        </div>
        <hr>
        <input v-model="book.name">
        <button class="primary" @click="submit">
          Button Label 2
        </button>
      </center>
    </div>
  </q-layout>
</template>

<script>
export default {
  data() {
    return {
      book: {
        'name': 'teste default'
      },
      books: []
    }
  },
  methods: {
    submit() {
      console.log(this.name)
      this.$http.post('http://localhost:8081/api/books', this.book).then(response => {
        console.log(response)
      }, error => {
        console.log(error)
      })
    }
  },
  created() {
    this.$http.get('http://localhost:8081/api/books').then(response => {
      //this.books = response;
      console.log('TESTE')
      console.log(response.body)
      this.books = response.body;
    }, error => {
      console.log(error)
    })
  }
}
</script>

<style lang="stylus">
</style>
