<template>
  <div>
    <!-- modal posts-->

    <h1 class="title">Post</h1>
    <table class="table is-striped">
      <thead>
        <tr>
          <th>Titulo</th>
          <th>Cuerpo</th>
          <th>&nbsp;</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in posts" :key="index">
          <td>{{item.title}}</td>
          <td>{{item.body}}</td>
          <td>
            <button @click="showModalClick(item.id)" class="button is-primary">ver comentarios</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- modal comments-->
    <div class="modal is-active" v-if="showModal">
      <div class="modal-background"></div>
      <div class="modal-content">
        <header class="modal-card-head">
          <p class="modal-card-title">Comentarios</p>
          <button class="delete" aria-label="close" @click="showModalClick(null)"></button>
        </header>
        <section class="modal-card-body">
          <table class="table is-striped">
            <thead>
              <tr>
                <th>nombre</th>
                <th>correo</th>
                <th>cuerpo</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in postSelected" :key="index">
                <td>{{item.name}}</td>
                <td>{{item.email}}</td>
                <td>{{item.body}}</td>
              </tr>
            </tbody>
          </table>
        </section>
      </div>
      <!----->
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "post-component",
  data: function() {
    return {
      posts: [],
      comments: [],
      postSelected: [],
      idPost: null,
      showModal: false
    };
  },
  methods: {
    getData() {
      return axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then(data => {
          this.posts = data.data;
        })
    },
    getComment() {
      return axios
        .get("https://jsonplaceholder.typicode.com/comments")
        .then(data => {
          this.comments = data.data;
        });
    },
    showModalClick(id) {
      if (this.showModal) {
        this.showModal = false;
        this.idPost = null;
        this.postSelected = [];
      } else {
        this.idPost = id;
        this.showModal = true;
        this.comments.forEach(item => {
          if (this.idPost === item.postId) {
            this.postSelected.push(item);
          }
        });
      }
    }
  },
  created() {
    this.getData();
    this.getComment();
  }
};
</script>
<style scoped>
</style>