# MEVN

a basic MEVN (MongoDB/Express/VueJS/Node.js) Stack application
https://medium.com/@anaida07/mevn-stack-application-part-1-3a27b61dcae0

### Install:
1. under main directory, create posts
2. install vue-cli :: npm install -g vue-cli
3. under posts, vue init webpack client
4. fill all questions 
	?Project name client
	? Project description A Vue.js project
	? Author rojaware <leesungki@gmail.com>
	? Vue build standalone
	? Install vue-router? Yes
	? Use ESLint to lint your code? Yes
	? Pick an ESLint preset Standard
	? Set up unit tests Yes
	? Pick a test runner karma
	? Setup e2e tests with Nightwatch? Yes
	? Should we run `npm install` for you after the project has been created? (recommended) npm
5.  cd client
	npm install
	npm run dev
6. open up the http://localhost:8080/#/in the browser which renders the default VueJS template.
7. prepare server in express
	mkdir server
	cd server
8. (create package.json on server directory, ) npm init -f
9. add 'start' command in package.json
	{
	  "name": "server",
	  "version": "1.0.0",
	  "description": "",
	  "main": "index.js",
	  "scripts": {
	    "start": "node src/app.js",
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "keywords": [],
	  "author": "",
	  "license": "ISC"
	}
10. create 'src' folder under 'server'
11. create 'app.js' under 'src'
    console.log('hello world');
12. (run server) npm start
13. (install dependencies for express under server) npm install express --save
14. (other dependencies) npm install --save body-parser cors morgan
15. update app.js as below
        const express = require('express')
		const bodyParser = require('body-parser')
		const cors = require('cors')
		const morgan = require('morgan')

		const app = express()
		app.use(morgan('combined'))
		app.use(bodyParser.json())
		app.use(cors())

		app.listen(process.env.PORT || 8081)

15. update app.js
	const express = require('express')
	const bodyParser = require('body-parser')
	const cors = require('cors')
	const morgan = require('morgan')

	const app = express()
	app.use(morgan('combined'))
	app.use(bodyParser.json())
	app.use(cors())

	app.get('/posts', (req, res) => {
	  res.send(
	    [{
	      title: "Hello World!",
	      description: "Hi there! How are you?"
	    }]
	  )
	})

	app.listen(process.env.PORT || 8081)
15. restart the server and visit http://localhost:8081/posts ( you should get json hello output on browser)
16. npm install --save nodemon

17. update package.json (under server) and replace start line with
    "scripts": {
      "start": "nodemon src/app.js" ,
      "test": "echo \"Error: no test specified\" && exit 1"
    },
18. (intall axios on client) go to client dir
19. npm install axios --save    
20. go to clients/src and , create new folder 'services'/ & add new file 'Api.js' , 
	import axios from 'axios'

	export default() => {
	  return axios.create({
	    baseURL: `http://localhost:8081`
	  })
	}
21. add new file 'PstsService.js' in the same location (client/src/services)
	import Api from '@/services/Api'

	export default {
	  fetchPosts () {
	    return Api().get('posts')
	  }
	}
22. go to src/router/index.js 	
	import Vue from 'vue'
	import Router from 'vue-router'
	import Hello from '@/components/Hello'
	import Posts from '@/components/Posts'

	Vue.use(Router)

	export default new Router({

	  routes: [
	    {
	      path: '/posts',
	      name: 'Posts',
	      component: Posts
	    },
	    {
	      path: '/',
	      name: 'Hello',
	      component: Hello
	    }
	  ],
	  mode: 'history'
	})
23. (add new view component) Posts.vue in src/components
	<template>
	  <div class="posts">
	    This file will list all the posts.
	  </div>
	</template>

	<script>
	export default {
	  name: 'posts',
	  data () {
	    return {}
	  }
	}
	</script>
23. (Test on server ) npm start, (on client) npm run dev under client , 
24. you will see new page when you go to http://localhost:8080/#/posts
25. (remove vue logo)
    open App.vue in client/src
    <img src="./assets/logo.png">
26. (update posts to list all posts) Posts.vue under client/src/components
27. You may have compilation error on Posts.vue around <div v-for="post in posts"> change to ==> <div v-bind:key="post in posts">
28. you go to the same location http://localhost:8080/posts , you will be able to see this:	
29. Update posts.vue
	<template>
	  <div class="posts">
	    <h1>Posts</h1>
	    This file will list all the posts.

	    <div v-for="post in posts">
	      <p>
	        <span><b>{{ post.title }}</b></span><br />
	        <span>{{ post.description }}</span>
	      </p>
	    </div>
	  </div>
	</template>

	<script>
	import PostsService from '@/services/PostsService'
	export default {
	  name: 'posts',
	  data () {
	    return {
	      posts: []
	    }
	  },
	  mounted () {
	    this.getPosts()
	  },
	  methods: {
	    async getPosts () {
	      const response = await PostsService.fetchPosts()
	      this.posts = response.data
	    }
	  }
	}
	</script>


### Run on Simple HTTP Server

To test build files locally, install a small http server globally:

npm install -g http-server

// then run in your projects directoy
http-server .
That allows you to test your build files locally.
Open browser http://localhost:8080/list.html
