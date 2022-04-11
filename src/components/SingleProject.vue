<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="flexing">
      <div>
        <h3 @click="showDetail = !showDetail">{{ project.title }}</h3>
      </div>
      <div class="icons">
       
        <router-link :to="{name:'editproject',params:{id:project.id}}">
             <span class="material-icons icon"> edit </span>
        </router-link>
        <span class="material-icons icon" @click="deleteProject"> delete </span>
        <span class="material-icons icon" @click="completeProject"> done </span>
      </div>
    </div>

    <p v-if="showDetail">{{ project.detail }}</p>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      api: "http://localhost:3000/projects",
    };
  },
  methods: {
    deleteProject() {
      let deleteRoute = this.api + "/" + this.project.id;
      fetch(deleteRoute, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    completeProject() {
      let updateCompleteRoute = this.api + "/" + this.project.id;
      fetch(updateCompleteRoute, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(
            {
                complete:!this.project.complete
            }
            ),
      }).then(()=>{
          this.$emit("completeUpdate",this.project.id);
      })
      .catch((err)=>{
          console.log(err);
      });
    },
  },
};
</script>

<style>
.project {
  padding: 20px;
  background-color: #f2f2f2;

  margin: 10px;
  border-radius: 10px;
  border-left: 6px solid crimson;
}
h3 {
  color: indigo;
  cursor: pointer;
}
.flexing {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
/* .icons{
    display: flex;
    justify-content: space-between;
} */
.icon {
  margin: 10px;
  cursor: pointer;
}
.icon:hover {
  color: #7777;
}
.complete {
  border-left: 6px solid lawngreen;
}
a{
    text-decoration: none;
}
</style>