<template>
  <div id="app">

    <div v-for="(el, index) in articles" :key="el.id">
      <Article :el="el" :articles='articles' :index="index" :comments="addArt.comments" />
    </div>

    <form action="">
        <label for="title"></label>
        <input
            type="text"
            id="title"
            v-model="addArt.title"
            class="form-control"
            placeholder="Заголовок"
        />
        <label for="shortDesc"></label>
        <textarea
            type="text"
            id="shortDesc"
            v-model="addArt.shortDesc"
            class="form-control"
            placeholder="Краткое описание"
        />
        <label for="longDesc"></label>
        <textarea
            type="text"
            id="longDesc"
            v-model="addArt.longDesc"
            class="form-control"
            placeholder="Полное описание"
        />
      </form>
    <div class="form-btn">
        <button class="btn btn-primary" @click="addArticle">Добавить запись</button>
    </div>
  </div>
</template>

<script>
import Article from './components/Article.vue'

export default {
  name: 'App',
  components: {
    Article
  },
  data () {
    return {
      addArt: {
        title: '',
        shortDesc: '',
        longDesc: '',
        comments: [],
      },
      articles: [],
    }
  },
  mounted () {
    this.getToStore()
  },
  methods: {
    getToStore(){
      let info = localStorage.getItem('articles')
      this.articles = JSON.parse(info)
    },
    addArticle () {
      let oldlist = localStorage.getItem('articles') || '[]'
      oldlist = JSON.parse(oldlist)

      const artInfo = JSON.stringify([...oldlist, this.addArt])

      localStorage.setItem('articles', artInfo)

      this.getToStore()
      
      this.addArt = {
        title: '',
        shortDesc: '',
        longDesc: '',
        comments: [],
      }
    }
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
  width: 500px;
  margin: 0 auto;
}

.form-btn {
  margin-top: 20px;
}

.btn-primary{
  background-color: #42b983;
}
</style>
