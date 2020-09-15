<template>
  <div class="hello">
    <!--<h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>-->
    <div class="todolist">
      <h3>Todo list JSON data fetched via Axios with basic auth</h3>
      -----
      <ul v-if="todos && todos.length">
        <li v-for="todo of todos" v-bind:key="todo">
          <p><strong>{{todo.id}}. {{todo.title}}</strong> - {{todo.description}}</p>
        </li>
      </ul>

      <ul v-if="errors && errors.length">
        <li v-for="error of errors" v-bind:key="error">
          {{error.message}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
//import {HTTP} from './http-common'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      todos: [],
      errors: []
    }
  },

  // Fetches posts when the component is created.
  created() {
    const api_url = process.env.VUE_APP_API_URL
    const username = process.env.VUE_APP_USERNAME
    const password = process.env.VUE_APP_PASSWORD
    const token = Buffer.from(`${username}:${password}`, 'utf8').toString('base64')
    /*const headers = {
      'Authorization': {username: 'jason', password:'decode'},
      'Content-Type': 'application/json',
    };*/
    axios.get(`${api_url}`, { 
        headers: {
          'Authorization': `Basic ${token}`
        }
      })
    .then(response => {
      // JSON responses are automatically parsed.
      this.todos = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  }

  /*created() {
    HTTP.get(`posts`)
    .then(response => {
      this.posts = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  }*/  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: list-item;
  margin: 0 10px;
}
a {
  color: #42b983;
}

/*.todolist {
  width: 20rem;
  text-align: left;
  margin: 0 30rem 0 30rem;
}*/
</style>
