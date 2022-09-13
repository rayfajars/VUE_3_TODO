<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"></FilterNav>
    <div v-if="projects.length">
      <!-- {{projects}} -->
      <div v-for="project in filterProjects" :about="project.id">
        <!-- <p>{{ project.title }}</p> -->
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        ></SingleProject>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";
// @ is an alias to /src

export default {
  name: "Home",
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDelete(id) {
      // console.log(id)
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      });
    },
    handleComplete(id) {
      // console.log(id)
      let p = this.projects.find((project) => {
        return project.id === id;
      });

      p.complete = !p.complete;
    },
  },
  computed: {
    filterProjects() {
      if (this.current === "completed") {
        return this.projects.filter((project) => project.complete);
      }
      if (this.current === "ongoing") {
        return this.projects.filter((project) => !project.complete);
      }

      return this.projects;
    },
  },
};
</script>
