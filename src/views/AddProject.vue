<template>
  <div>
    <form action="" @submit.prevent="SaveData">
      <label for="">Title</label>
      <input v-model="Item.title" type="text" />
      <input v-model="Item.complete" type="checkbox" />
      <label for="">Details</label>
      <textarea v-model="Item.details" cols="30" rows="10"></textarea>
      <button @submit="SaveData" class="mt-10">Save</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "AddNewProject",
  data() {
    return {
      Item: {},
      ID: null,
    };
  },
  mounted() {
    console.log();
    this.ID = this.$route.params;
    debugger;
    this.EditData(this.ID.id);
  },
  methods: {
    async EditData(ID) {
      await fetch("http://localhost:3000/projects/" + ID)
        .then((e) => e.json())
        .then((e) => (this.Item = e));
    },
    SaveData() {
      console.log(this.Item);
      if (this.ID) {
        fetch("http://localhost:3000/projects/" + this.ID.id, {
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            details: this.Item.details,
            complete: this.Item.complete,
            title: this.Item.title,
          }),
        }).then(() => {
          this.$router.push("/");
        });
      } else {
        fetch("http://localhost:3000/projects/", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.Item),
        }).then(() => this.$router.push("/"));
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
div {
  display: flex;
  flex-direction: row;
}
form {
  margin: 20px;
  padding: 20px;
  background-color: rgb(17, 255, 0);
}
input {
  box-sizing: border-box;
}
.mt-10 {
  margin-top: 10px;
}
</style>
