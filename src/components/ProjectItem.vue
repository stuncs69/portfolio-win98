<template>
  <div class="project-navigation">
    <button class="win98-button nav-button" @click="prevProject">&lt;</button>
    <div class="win98-inset project-item">
      <div class="project-title">{{ currentProject.title }}</div>
      <div class="project-description">{{ currentProject.description }}</div>
      <div class="project-links">
        <a v-for="(link, index) in currentProject.links" 
           :key="index" 
           :href="link.url" 
           class="win98-link"
           target="_blank">
          {{ link.name }}
          <span v-if="index < currentProject.links.length - 1" class="link-separator">|</span>
        </a>
      </div>
    </div>
    <button class="win98-button nav-button" @click="nextProject">&gt;</button>
  </div>
</template>

<script>
export default {
  props: {
    projects: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      currentIndex: 0
    };
  },
  computed: {
    currentProject() {
      return this.projects[this.currentIndex];
    }
  },
  methods: {
    nextProject() {
      if (this.currentIndex < this.projects.length - 1) {
        this.currentIndex++;
      } else {
        this.currentIndex = 0;
      }
    },
    prevProject() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
      } else {
        this.currentIndex = this.projects.length - 1;
      }
    }
  }
};
</script>

<style scoped>
.project-links {
  margin-top: 8px;
}

.win98-link {
  color: #0000ff;
  text-decoration: underline;
  cursor: pointer;
}

.win98-link:hover {
  color: #ff0000;
}

.link-separator {
  margin: 0 5px;
  color: #000000;
}

.project-title {
  font-weight: bold;
  margin-bottom: 5px;
}

.project-description {
  margin-bottom: 8px;
}
</style>