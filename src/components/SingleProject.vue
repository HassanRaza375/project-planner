<template>
  <div id="div-0" :class="Project.complete ? 'border-left' : ''">
    <div id="div-1">
      <span id="cursor" @click="show = !show">ID = {{ Project.id }}</span>
      <div class="d-flex justify-end">
        <span
          class="cursor material-symbols-rounded"
          @click="CompleteRecord(Project.id)"
        >
          done
        </span>
        <span
          class="cursor material-symbols-rounded"
          @click="DeleteRecord(Project.id)"
        >
          delete
        </span>
        <router-link :to="'/AddProject/' + Project.id">
          <span class="cursor material-symbols-rounded"> edit</span>
        </router-link>
      </div>
      <div id="div-2">
        <p>Title = {{ Project.title }}</p>
        <hr />
        <p v-if="show">Details = {{ Project.details }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SP",
  props: {
    Project: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      show: false,
    };
  },
  mounted() {
    console.log(this.Project);
  },
  methods: {
    CompleteRecord(ID) {
      fetch("http://localhost:3000/projects/" + ID, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.Project.complete }),
      }).then(() => this.$emit("Complete", ID));
    },
    DeleteRecord(ID) {
      fetch("http://localhost:3000/projects/" + ID, { method: "Delete" }).then(
        () => this.$emit("delete", ID)
      );
    },
  },
};
</script>

<style scoped>
#cursor {
  cursor: pointer;
}
.cursor {
  cursor: pointer;
}
#div-0 {
  background-color: #fff;
  border-radius: 5px;
  border-left: 5px solid rgb(247, 64, 134);
}
.border-left {
  border-left: 6px solid lime !important;
}
#div-1 {
  padding: 10px;
  margin: 10px;
}
.d-flex {
  display: flex;
}
.justify-end {
  justify-content: end;
}
</style>
