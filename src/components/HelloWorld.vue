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

      <ul v-if="getErrors && getErrors.length">
        <li v-for="getError of getErrors" v-bind:key="getError">
          {{getError.message}}
        </li>
      </ul>
      -----
      
      <ul v-if="errors && errors.length">
        <li v-for="error of errors" v-bind:key="error">
          {{error.message}}
        </li>
      </ul>
      <div class="container">
      <div class="todo-form">
        <form @submit.prevent="onSubmit" class="add-item-form">
          <div class="input">
            <label for="title">Title</label>
            <input
               type="text"
               id="title"
               v-model="title" 
               required>
          </div>
          <div class="input">
            <label for="name">Description</label>
            <input
              type="text"
              id="description"
              v-model="description"
              required>
          </div>
          <div class="input">
            <label for="finished">Finished?</label>
            <input
              type="text"
              id="finished"
              v-model="finished"
              required>
          </div>
          <div class="submit">
            <button class="btn" type="submit">Submit</button>
          </div>
        </form>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
//import qs from 'qs';
//import {HTTP} from './http-common'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      todos: [],
      getErrors: [],
      postBody: '',
      formData: ''
    }
  },

  // Fetches posts when the component is created.
  created() {
    const api_url = process.env.VUE_APP_API_URL
    const username = process.env.VUE_APP_USERNAME
    const password = process.env.VUE_APP_PASSWORD
    const token = Buffer.from(`${username}:${password}`, 'utf8').toString('base64')
    //add const headers here if not using env
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
      this.getErrors.push(e)
    })
  },

  /*created() {
    HTTP.get(`posts`)
    .then(response => {
      this.posts = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  }*/
  // Pushes todos to the server when called
  methods: {
    onSubmit() {
      const api_url = process.env.VUE_APP_API_URL
      const username = process.env.VUE_APP_USERNAME
      const password = process.env.VUE_APP_PASSWORD
      const token = Buffer.from(`${username}:${password}`, 'utf8').toString('base64')
      const formData = {
      title: this.title,
      description: this.description,
      finished: this.finished      
      }
      axios({
        method: "POST",
        url: `${api_url}`,
        headers: {
          "Authorization": `Basic ${token}`,
          "Content-Type": "application/json"
        },
        data: formData
      })
      .then(response => { 
        console.log(response);
      })
      .catch(e => {
      this.errors.push(e)
      })
    },
    
  }
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

.container {
    font-family: system-ui, BlinkMacSystemFont, -apple-system, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    padding: 0;
}

.todo-form {
    background: #FFF;
    padding: 2rem;
    margin: 1rem;
    border-radius: 3px;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.12), 0 2px 4px 0 rgba(0,0,0,0.08);
    width: 50%;
    max-width: 900px;
}

.todo-form > input, .todo-form > select {
    width: 100%;
    border-radius: 3px;
    box-shadow: 0 2px 4px 0 rgba(0,0,0,0.10);
    border: 1px solid #F1F5F8;
    color: #606F7B;
    padding: .5rem .75rem;
    box-sizing: border-box;
    font-size: 1rem;
    letter-spacing: .5px;
    margin: .5rem 0;
    text-align: left;
}

.add-item-form, .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.add-item-form input label {
    width: 70%;
    border-radius: 3px;
    box-shadow: 0 2px 4px 0 rgba(0,0,0,0.10);
    border: 1px solid #F1F5F8;
    color: #606F7B;
    padding: .5rem .75rem;
    box-sizing: border-box;
    font-size: 1rem;
    letter-spacing: .5px;
    margin: .5rem 0;
}

.btn {
    border: none;
    border-radius: 3px;
    margin: auto 0;
    padding: .5rem .75rem;
    flex-shrink: 0;
    cursor: pointer;
    font-size: .9rem;
    letter-spacing: .5px;
    transition: all .1s ease-in;
}

.btn[disabled] {
    background: #8795A1;
}

.btn[disabled]:hover {
    background: #606F7B;
}

.btn-primary {
    background: #6CB2EB;
    color: #fff;
}

.btn-primary:hover {
    background: #3490DC;
}

.btn-cancel {
    background: #EF5753;
    color: #fff;
}

.btn-cancel:hover {
    background: #E3342F;
    color: #fff;
}
</style>
