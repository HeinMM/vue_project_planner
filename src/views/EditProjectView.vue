<template>
  <h1>Edit{{ id }}</h1>
  <form @submit.prevent="updateProject">
    <label for="">Project Title</label>
    <input type="text" v-model="title" />
    <label for="">Project Detail</label>
    <input type="text" v-model="detail" />
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      detail: "",
    };
  },
  methods: {
    updateProject() {
      fetch("http://localhost:3000/projects/" + this.id, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
         body: JSON.stringify({
            title:this.title,
            detail:this.detail,
        })
      }).then(
          this.$router.push({name:'home'})
      )
      .catch((err)=>{
          console.log(err);
      });
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects/" + this.id)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.title = data.title;
        this.detail = data.detail;
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style>
</style>