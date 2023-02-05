<template>
  <div class="home">
    <ul>
      <li v-for="user in User" :key="user.id">
        <SingleProject
          :Project="user"
          @delete="deleteHandler"
          @Complete="CompleteHandler"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
export default {
  name: "HomeView",
  components: { SingleProject },
  data() {
    return {
      User: [],
    };
  },
  async mounted() {
    await fetch("http://localhost:3000/projects")
      .then((e) => e.json())
      .then((e) => (this.User = e));
    console.log(this.User);
  },
  methods: {
    CompleteHandler(id) {
      let p = this.User.find((e) => {
        return (e.id = id);
      });
      p.complete = !p.complete;
      debugger;
    },
    deleteHandler(Id) {
      this.User = this.User.filter((e) => e.id !== Id);
      console.log(this.User);
      debugger;
    },
  },
};
</script>
