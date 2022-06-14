<template>
  <div class="hello">
    <input 
    v-model="title" id="title"
    type="text">
    <input 
    v-model="content" id="content"
    type="text">
    <input 
    v-model="user" id="user"
    type="text">
    <input 
    v-model="created_at" id="created_at"
    type="text">

    <button 
    @click="postBlogPost"
    >POST</button>
    <button 
    @click="getPosts"
    >GET</button>

    <div
    v-for="post in postArr"
    :key="post.animalId"
    >
      {{post.postId}}
      <h1>{{post.title}}</h1>
      <p>{{post.content}}</p>
      <h3>{{post.user}}</h3>
      <h3>{{post.created_at}}</h3>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data() {
    return {
      content: null,
      title: null,
      created_at: null,
      user: null,
      postArr: []
    }
  },
  methods: {
    getPosts(){
      console.log("Running GET");
      axios.request({
        url : 'http://127.0.0.1:5000/api/posts',
        method : "GET"
      }).then((response)=>{
        console.log(response);
        this.postArr = response.data;
      }).catch((error)=>{
        console.log(error);
      })
    },
    postBlogPost() {
      axios.request({
        url : 'http://127.0.0.1:5000/api/posts',
        method : "POST",
        data : {
          content : this.content,
          title : this.title,
          user : this.user,
          created_at : this.created_at
        },
      }).then(()=>{
        this.getPosts();
      }).catch((error)=>{
        console.log(error);
      })
      console.log(this.content, this.title, this.user, this.created_at);
    },
  },
  mounted () {
    this.getPosts();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
