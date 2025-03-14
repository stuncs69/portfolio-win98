<template>
  <div v-if="isVisible" class="modal-overlay" @click.self="closeModal">
    <div class="win98-window modal-window">
      <div class="win98-titlebar">
        <div class="win98-titlebar-text">Contact</div>
        <div class="win98-titlebar-buttons">
          <button class="win98-button close" @click="closeModal">×</button>
        </div>
      </div>
      <div class="win98-window-content">
        <div class="contact-content">
          <p class="contact-heading">You can find me on GitHub:</p>
          <div class="github-card">
            <a href="https://github.com/stuncs69" target="_blank" class="win98-link github-link">
              <div class="github-avatar">
                <img src="https://avatars.githubusercontent.com/u/68465860" alt="GitHub Avatar" />
              </div>
              <div class="github-username">github.com/stuncs69</div>
            </a>
          </div>
        </div>
      </div>
      <div class="win98-statusbar">
        <div class="win98-statusbar-section">Ready</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    isVisible: {
      type: Boolean,
      required: true,
      default: false
    }
  },
  emits: ['close'],
  methods: {
    closeModal(): void {
      this.$emit('close');
    }
  }
});
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-window {
  width: 380px;
  position: relative;
}

.contact-content {
  padding: 20px;
  text-align: center;
}

.contact-heading {
  font-size: 14px;
  margin-bottom: 15px;
}

.github-card {
  border: 2px solid;
  border-top-color: var(--win98-inset-light);
  border-left-color: var(--win98-inset-light);
  border-right-color: var(--win98-inset-dark);
  border-bottom-color: var(--win98-inset-dark);
  background-color: white;
  padding: 15px;
  margin: 0 auto;
  max-width: 280px;
}

.github-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  text-decoration: none;
  color: #0000ff;
}

.github-link:hover .github-username {
  color: #ff0000;
  text-decoration: underline;
}

.github-avatar {
  width: 100px;
  height: 100px;
  border: 2px solid;
  border-top-color: var(--win98-button-dark-shadow);
  border-left-color: var(--win98-button-dark-shadow);
  border-right-color: var(--win98-button-highlight);
  border-bottom-color: var(--win98-button-highlight);
  padding: 3px;
  margin-bottom: 10px;
  background-color: white;
}

.github-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.github-username {
  font-size: 14px;
  margin-top: 5px;
}
</style>