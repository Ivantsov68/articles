<template>
   <div class="wrapper">

      <form action="">
         <label for="Name"></label>
         <input
            type="text"
            id="Name"
            v-model="comments.userName"
            class="form-control"
            placeholder="Имя пользователя"
         />
         <label for="TextComment"></label>
         <textarea
            type="text"
            id="TextComment"
            v-model="comments.textComment"
            class="form-control"
            placeholder="Комментарий"
         />
      </form>
      <div class="form-btn">
         <button class="btn btn-primary" @click="add">Добавить Комментарий</button>
      </div>

   </div>
</template>

<script>

export default {
   name: 'Comment',
   props: [
      'index',
      'articles'
   ],
   data() {
      return {
         comments: {
            userName: '',
            textComment: ''
         }
      }
   },
   methods: {
      add() {
         let allArticles = localStorage.getItem('articles')
         allArticles = JSON.parse(allArticles)
         allArticles[this.index].comments = [this.comments, ...allArticles[this.index].comments]

         const comInfo = JSON.stringify(allArticles.sort(function(a, b) {
            return a - b;
         }))
         
         localStorage.setItem('articles', comInfo)

         this.articles[this.index].comments = JSON.parse(localStorage.getItem('articles'))[this.index].comments

         this.comments = {
            userName: '',
            textComment: ''
         }
      }
   }
}

</script>

<style scoped>

.form-btn {
   margin: 15px 0;
   display: flex;
   justify-content: space-around;
}

</style>