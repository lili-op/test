<template>
  <div id="app">
    <h1>我的博客</h1>
    <comment-form @add-comment="doAdd" />
    <comment-list title="热门评论" :comments="comments" @delete-comment="doDelete" />
  </div>
</template>

<script>
 import CommentForm from './components/comment-form.vue';
 import CommentList from './components/comment-list.vue';
 import axios from 'axios';

 export default {
   name: 'App',
   data() {
     return  {
       comments: []
     }
   },
   methods: {
     loadcomment(){
       axios({
         url: "/api/comments",
         responseType: "json"
       }).then(data =>  {
         this.comments = data.data;
       })
     },
     doDelete(b) {
       axios.get("/api/comment/del?id=" + b).then(resp =>  {
         this.loadcomment();
       });
     },
     doAdd(comment) {
       console.log(comment)
       axios({
         method: 'post',
         url: '/api/comment/add?author='+comment.author+'&body='+comment.body,
       }).then(resp => {
         this.loadcomment();
         comment = "";
       });
     },
   },
mounted () {
  this.loadcomment();
},
   components: {
     CommentForm, CommentList
   }
 }
</script>

<style>
 #app {
   font-family: Avenir, Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   margin-top: 60px;
 }
</style>