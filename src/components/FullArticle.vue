<template>
   <div class="wrapper">
      <div class="article-wrapper">

         <h1>{{el.title}}</h1>
         <p class="long-desc">{{el.longDesc}}</p>
         <p class="quantity">Количество комментариев: {{articles[index].comments.length}}</p>
         <div class="line"></div>

         <div v-for="(el, index) of articles[index].comments" :key="index">

            <div class="wrapper-comment" :name="index">
               <a href="" class="delete-art" @click.prevent="deleteComment">&#10006;</a>
               <p class="username">{{el.userName}}</p>
               <p class="text">{{el.textComment}}</p>
            </div>

         </div>

         <Comment :articles='articles' :index='index'/>
         
      </div>
   </div>
</template>

<script>

import Comment from './Comment'

export default {
   name: "FullArticle",
   components: {
      Comment
   },
   props: [
      'index',
      'articles',
      'el'
   ],
   methods: {
      deleteComment(e) {
         const commentID = +e.target.parentNode.getAttribute('name')

         let del = localStorage.getItem('articles')
         del = JSON.parse(del)
         del[this.index].comments.splice(commentID, 1)

         localStorage.setItem('articles', JSON.stringify(del))

         this.articles[this.index].comments.splice(commentID, 1) 
      }
   }
};
</script>

<style scoped>

   .long-desc, .quantity {
      font-size: 20px;
   }

   .wrapper-comment {
      margin-top: 15px;
      display: flex;
      flex-direction: column;
   }

   .username {
      text-align: start;
      text-decoration: underline;
   }

   .text {
      text-align: start;
   }

   .delete-art {
      align-self: flex-end;
      color: #42b983;
   }

   .line{
      border-bottom: 1px solid #000;
   }
   
</style>