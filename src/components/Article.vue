<template>
  <div class="wrapper">
    <div class="article-wrapper">
      
      <div class="edit-wrapper">
        <a href="" class="edit-art" @click.prevent="zoom">ред.</a>
        <a href="" class="delete-art" @click.prevent="deleteArticle">&#10006;</a>
      </div>

      <div v-if="!open">
        <h1>{{ el.title }}</h1>
        <p class="short-desc">
          {{ el.shortDesc }}
        </p>
        <p class="quantity">
          Количество комментариев: {{ articles[index].comments.length }}
        </p>
      </div>

      <div class="wrapper-content" v-if="open">
        <FullArticle :el="el" :articles="articles" :index="index" />
      </div>

      <div id="myModal" class="modal">
        <span class="close">&times;</span>

        <img class="modal-content" id="img01" />

        <div id="caption">
          <form action="">
            <label for="title"></label>
            <input
              type="text"
              id="title"
              v-model="editArt.title"
              class="form-control"
              placeholder="Заголовок"
            />
            <label for="shortDesc"></label>
            <textarea
              type="text"
              id="shortDesc"
              v-model="editArt.shortDesc"
              class="form-control"
              placeholder="Краткое описание"
            />
            <label for="longDesc"></label>
            <textarea
              type="text"
              id="longDesc"
              v-model="editArt.longDesc"
              class="form-control"
              placeholder="Полное описание"
            />

            <div class="form-btn">
              <button class="btn btn-primary" @click.prevent="saveArticle">Сохранить</button>
            </div>
          </form>
        </div>
      </div>

      <a href="" class="more" @click.prevent="moreInfo">{{open ? "Скрыть" : "Узнать больше"}}</a>
    </div>
  </div>
</template>

<script>
import FullArticle from "./FullArticle";

export default {
  name: "Article",
  components: {
    FullArticle,
  },
  props: ["el", "index", "articles"],
  data() {
    return {
      open: false,
      editArt: { ...this.articles[this.index] },
    };
  },
  methods: {
    saveArticle(e) {
      const less = {
        ...this.editArt,
        comments: this.articles[this.index].comments,
      };

      let store = JSON.parse(localStorage.getItem("articles"));
      store.splice(this.index, 1, less);
      localStorage.setItem("articles", JSON.stringify(store));

      this.articles.splice(this.index, 1, less);

      let modal = e.target.parentNode.parentNode.parentNode.parentNode;
      modal.style.display = "none";
    },
    moreInfo() {
      this.open = !this.open;
    },
    deleteArticle() {
      let del = localStorage.getItem("articles");
      del = JSON.parse(del);
      del.splice(this.index, 1);
      localStorage.setItem("articles", JSON.stringify(del));
      this.articles.splice(this.index, 1);
    },

    zoom(e) {
      let modal = e.target.parentNode.parentNode.querySelector("#myModal");
      modal.style.display = "block";

      let span = e.target.parentNode.parentNode.querySelector(".close");

      span.onclick = function () {
        modal.style.display = "none";
      };
    },
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
  color: #42b983;
}

.article-wrapper {
  border: 1px solid black;
  margin: 20px 0;
  border-radius: 2%;
  padding: 20px;
}

.short-desc,
.quantity {
  font-size: 20px;
}

.edit-wrapper {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.btn-primary{
  background-color: #42b983;
}

.modal {
  display: none;
  position: fixed;
  z-index: 1;
  padding-top: 100px;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.9);
}
.modal-content {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 700px;
}

#caption {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 700px;
  text-align: center;
  color: #ccc;
  padding: 10px 0;
  height: 150px;
}

.modal-content,
#caption {
  animation-name: zoom;
  animation-duration: 0.6s;
}

@keyframes zoom {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}
.close {
  position: absolute;
  top: 15px;
  right: 35px;
  color: #f1f1f1;
  font-size: 40px;
  font-weight: bold;
  transition: 0.3s;
}
.close:hover,
.close:focus {
  color: #bbb;
  text-decoration: none;
  cursor: pointer;
}

</style>
