<template>
  <section>
    <b-field label='Name'>
      <b-input v-model='newTodo'></b-input>
    <div class="control">
      <button class="button is-link" @click='addTodo()'>Submit</button>
    </div>
  </b-field>
  <div v-for="(todo, index) in todos" :key="index">
    <b-field class="is-pulled-left handle">
        <label class="subtitle is-3">
          {{todo.text}}
          <div v-if ='todo.done'>
            <input type="text" v-model="todo.text">
            <a class="button is-text" @click='updateTodo(todo, todo.text)'>edit</a>
     </div>
        </label>
      </b-field>
      <a class="delete is-pulled-right is-medium" @click="removeTodo(todo)"></a>
      <div><a class="button is-text" @click='editTodo(todo)'>Text</a></div>
      <div class="is-clearfix"></div>
 </div>
  </section>
</template>

<script>
import Firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCzuQaQrDmuXYqZ3M-hA_sUhhyODvvAtk0',
  authDomain: 'testproject-45b68.firebaseapp.com',
  databaseURL: 'https://testproject-45b68.firebaseio.com',
  projectId: 'testproject-45b68',
  storageBucket: 'testproject-45b68.appspot.com',
  messagingSenderId: '498111622733'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let todosRef = db.ref('todos')
export default {
  name: 'HelloWorld',
  firebase: {
    todos: todosRef
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newTodo: '',
      deleteTodo: ''
    }
  },
  methods: {
    addTodo () {
      todosRef.push({
        text: this.newTodo,
        done: false
      })
      this.newTodo = ''
    },
    removeTodo (todo) {
      todosRef.child(todo['.key']).remove()
    },
    updateTodo (todo, text) {
      todosRef.child(todo['.key'] + '/text').set(text)
      todosRef.child(todo['.key'] + '/done').set(false)
    },
    editTodo (todo) {
      todosRef.child(todo['.key'] + '/done').set(true)
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
