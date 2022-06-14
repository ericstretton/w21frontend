<template>
  <div class="hello">
    <div>
      <h1>Make a New Post</h1>
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
    </div>
    
    <div>
      <h1>Update a Post</h1>
      <input 
      v-model="title" id="updatetitle"
      type="text">
      <input 
      v-model="content" id="updatecontent"
      type="text">
      <input 
      v-model="postId" id="updateid"
      type="text">
    </div>

    <div>
      <h1>Delete a Post</h1>
      <input 
      v-model="postId" id="deleteid"
      type="text">
    </div>

    <button 
    @click="postBlogPost"
    >POST</button>
    <button 
    @click="getPosts"
    >GET</button>
    <button 
    @click="updatePost"
    >UDATE</button>
    <button 
    @click="deletePost"
    >DELETE</button>



    <div
    v-for="post in postArr"
    :key="post.animalId"
    class="posts"
    >
    <div>
      {{post.postId}}
      <h1>{{post.title}}</h1>
      <p>{{post.content}}</p>
      <h3>{{post.user}}</h3>
      <h3>{{post.created_at}}</h3>
      </div>
      
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
      postId : null,
      postArr: []
    }
  },
  methods: {
    getPosts(){
      console.log("Running GET");
      axios.request({
        url : VUE_APP_API_URL + "/posts",
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
        url : VUE_APP_API_URL + "/posts",
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
    updatePost(content, title, postId) {
      axios.request({
        url : VUE_APP_API_URL + "/posts",
        method : "PATCH",
        params : {
          content, title, postId
        },
        data : {
          content : this.content,
          title : this.title,
          postId : this.postId
        }
      }).then(()=>{
        this.getPosts();
      }).catch((error)=> {
        console.log(error);
      })
    },
    deletePost(postId) {
      axios.request({
        url : VUE_APP_API_URL + "/posts",
        method : "DELETE",
        params: postId,
        data : {
          postId : this.postId
        }
      }).then(()=> {
        this.getPosts();
      }).catch((error)=> {
        console.log(error);
      })
    }
  },
  mounted () {
    this.getPosts();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello{
  background-color: bisque;
  .posts{
    margin: 15px;
    width: 70vw;
    position: relative;
    left : 12vw;
  }
}
</style>
