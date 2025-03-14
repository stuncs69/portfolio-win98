<template>
  <div class="win98-desktop">
    <div class="win98-window" ref="windowRef">
      <div class="win98-titlebar" >
        <div class="win98-titlebar-text">asahi.zip</div>
        <div class="win98-titlebar-buttons">
          <button class="win98-button minimize">_</button>
          <button class="win98-button maximize">□</button>
          <button class="win98-button close">×</button>
        </div>
      </div>
      <div class="win98-window-content">
        <div class="ascii-art">
          <pre>
       |\      _,,,---,,_
ZZZzz /,`.-'`'    -.  ;-;;,_
     |,4-  ) )-,_. ,\ (  `'-'
    '---''(_/--'  `-'\_)  asahi.zip
          </pre>
        </div>

        <div class="win98-section">
          <div class="win98-section-title">
            <div class="win98-inset-border">Projects</div>
          </div>
          
          <ProjectItem :projects="projects" />
        </div>

        <div class="win98-section">
          <div class="win98-section-title">
            <div class="win98-inset-border">Approved Music</div>
          </div>
          
          <MusicItem :albums="albums" />
        </div>

        <div class="contact-section">
          <button class="win98-button contact-button" @click="showContactModal = true">Contact</button>
        </div>
        
        <ContactModal :is-visible="showContactModal" @close="showContactModal = false" />

        <div class="footer">
          <div class="duck-icon">🦆</div>
        </div>
      </div>
      <div class="win98-statusbar">
        <div class="win98-statusbar-section">Ready</div>
        <div class="win98-statusbar-section">CPU: 3%</div>
        <div class="win98-statusbar-section">RAM: 32MB / 64MB</div>
        <div class="win98-statusbar-section">
           <a class="win98-link" href="https://github.com/stuncs69/portfolio-win98" target="_blank">Source</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import ProjectItem from './components/ProjectItem.vue';
import MusicItem from './components/MusicItem.vue';
import ContactModal from './components/ContactModal.vue';
import { defineComponent } from 'vue'

interface Position {
  x: number;
  y: number;
}

interface WindowPosition {
  top: string;
  left: string;
  transform: string;
}

interface Album {
  title: string;
  artist: string;
  image: string;
  link: string;
}

interface ProjectLink {
  name: string;
  url: string;
}

interface Project {
  title: string;
  description: string;
  links: ProjectLink[];
}

export default defineComponent({
  components: {
    ProjectItem,
    MusicItem,
    ContactModal
  },
  data() {
    return {
      isDragging: false,
      initialMousePosition: { x: 0, y: 0 } as Position,
      currentMousePosition: { x: 0, y: 0 } as Position,
      initialWindowPosition: { x: 0, y: 0 } as Position,
      windowPosition: { top: '50%', left: '50%', transform: 'translate(-50%, -50%)' } as WindowPosition,
      animationFrameId: null as number | null,
      lastUpdateTime: 0,
      THROTTLE_DELAY: 5,
      showContactModal: false,
      albums: [
        {
          title: "Vivid",
          artist: "Liquid Stella",
          image: "https://i.scdn.co/image/ab67616d00001e02eaacc3fff71f62cf8e37a3b0",
          link: "https://open.spotify.com/album/3etinD27F1y6Oxb0NLcqav?si=1_YfWzQzR26a55FtI8kQaQ"
        },
        {
          title: "云う透り e.p",
          artist: "JYOCHO",
          image: "https://i.scdn.co/image/ab67616d0000b2732b31301a1ffc229aaf7e5279",
          link: "https://open.spotify.com/album/5Ng3rikw2a7AWG0bbK9ccN?si=tNKkpN5-Tp-vlqPXlc2pdQ"
        },
        {
          title: "華火夜景",
          artist: "bohemianvoodoo",
          image: "https://i.scdn.co/image/ab67616d00001e020ce843552195efba999847f8",
          link: "https://open.spotify.com/album/57hUpIDR3di336V6dO6U8v?si=uiL6gHlZQ8qOE3LncHr6ow"
        },
        {
          title: "Metaphor",
          artist: "Ichika Nito",
          image: "https://i.scdn.co/image/ab67616d00001e0287a6bcdbe4ac5b5e5a3d5069",
          link: "https://open.spotify.com/album/7yQhaIIs7VV60pO79RGKJR?si=B2sr935tQ_OK1GCjfuvL5w"
        },
        {
          title: "角度 Angle",
          artist: "Elephant Gym",
          image: "https://i.scdn.co/image/ab67616d00001e0287777bb747d3916b94fcdbd4",
          link: "https://open.spotify.com/album/61PntG5EJgzniCaVz1TjTE?si=4-L-4TAOQQa8LuMG-kwYIw"
        },
        {
          title: "Remember That You Will Die",
          artist: "Polyphia",
          image: "https://i.scdn.co/image/ab67616d00001e02a2d8391f5021568d253a4eef",
          link: "https://open.spotify.com/album/3cN3mENkACWuRCDOuQUtfw?si=n_9MjCPrQKOuesULs2jNiA"
        },
        {
          title: "Scherzo Eterno",
          artist: "Stegosauro",
          image: "https://i.scdn.co/image/ab67616d00001e0216756424b639435e1b5d0ec9",
          link: "https://open.spotify.com/album/0DKGfiSJHH4rwwxOe389As?si=kZoVwHTMTtyiiAViA5sJUA"
        },
        {
          title: "capture the Initial \"F\"(2021 NEW TAKE)",
          artist: "fox capture plan",
          image: "https://i.scdn.co/image/ab67616d00001e02cad99adab9b2566c2f9974ee",
          link: "https://open.spotify.com/album/55GK1qonnl778t4XDYjX3p?si=QmoIqhmiSh-LenkeSa2iTw"
        }
      ] as Album[],
      projects: [
        {
          title: 'Vexel',
          description: 'JIT-compiled scripting language with temper.',
          links: [
            { name: 'GitHub', url: 'https://github.com/stuncs69/vexel' },
            { name: 'docs', url: 'https://vexel.asahi.zip/' }
          ]
        },
        {
          title: 'Tidepool',
          description: 'TUI (Terminal user-interface) library for TypeScript.',
          links: [
            { name: 'GitHub', url: 'https://github.com/stuncs69/tidepool' },
            { name: 'NPM', url: 'https://www.npmjs.com/package/tidepool' }
          ]
        },
        {
          title: 'Papaya.js',
          description: 'View-controller OOP backend framework for TypeScript.',
          links: [
            { name: 'GitHub', url: 'https://github.com/stuncs69/papaya.js' },
            { name: 'NPM', url: 'https://www.npmjs.com/package/papaya.js' }
          ]
        },
        {
          title: 'MK Builder',
          description: 'Mario Kart 8 Deluxe build calculator',
          links: [
            { name: 'website', url: 'https://mk.asahi.zip/' }
          ]
        },
        {
          title: 'Minecraft Server',
          description: 'Whitelist-only at mc.asahi.zip',
          links: []
        },
        {
          title: 'Custom bots',
          description: 'I develop and maintain a few bots ranging from gateway systems to automatic chat moderation.',
          links: []
        }
      ] as Project[]
    };
  },
  computed: {
  }
});
</script>

<style>
body {
  margin: 0;
  display: flex;
  place-items: center;
  min-width: 100svw;
  min-height: 100svh;
}

html {
  background-color: #008080;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  text-align: center;
}

:root {
  --win98-bg: #c0c0c0;
  --win98-window-bg: #c0c0c0;
  --win98-titlebar: #000080;
  --win98-titlebar-text: #ffffff;
  --win98-button-face: #c0c0c0;
  --win98-button-highlight: #ffffff;
  --win98-button-shadow: #808080;
  --win98-button-dark-shadow: #000000;
  --win98-text: #000000;
  --win98-inset-light: #808080;
  --win98-inset-dark: #ffffff;
}

* {
  box-sizing: border-box;
  font-family: 'MS Sans Serif', 'Tahoma', sans-serif;
  font-size: 12px;
  user-select: none;
}

.win98-desktop {
  background-color: #008080;
  min-height: 100vh;
  padding: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.win98-window {
  width: 600px;
  background-color: var(--win98-window-bg);
  border: 2px solid;
  border-top-color: var(--win98-button-highlight);
  border-left-color: var(--win98-button-highlight);
  border-right-color: var(--win98-button-dark-shadow);
  border-bottom-color: var(--win98-button-dark-shadow);
  box-shadow: 1px 1px 0 1px var(--win98-button-shadow);
  position: absolute;
}

.win98-titlebar {
  background-color: var(--win98-titlebar);
  color: var(--win98-titlebar-text);
  padding: 2px 3px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: move;
}

.win98-titlebar-text {
  font-weight: 500;
  padding-left: 3px;
}

.win98-titlebar-buttons {
  display: flex;
}

.win98-button {
  border: 2px solid;
  border-top-color: var(--win98-button-highlight);
  border-left-color: var(--win98-button-highlight);
  border-right-color: var(--win98-button-dark-shadow);
  border-bottom-color: var(--win98-button-dark-shadow);
  background-color: var(--win98-button-face);
  box-shadow: 1px 1px 0 0 var(--win98-button-shadow);
  padding: 0 4px;
  min-width: 16px;
  min-height: 16px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-left: 2px;
  font-size: 10px;
  line-height: 1;
  cursor: pointer;
}

.win98-button:active {
  border-top-color: var(--win98-button-dark-shadow);
  border-left-color: var(--win98-button-dark-shadow);
  border-right-color: var(--win98-button-highlight);
  border-bottom-color: var(--win98-button-highlight);
  box-shadow: 1px 1px 0 0 var(--win98-button-shadow) inset;
}

.win98-window-content {
  padding: 10px;
  background-color: var(--win98-window-bg);
}

.win98-statusbar {
  border-top: 1px solid var(--win98-button-shadow);
  padding: 2px 4px;
  display: flex;
  font-size: 11px;
}

.win98-statusbar-section {
  margin-right: 10px;
  padding: 2px 5px;
  border: 1px solid;
  border-top-color: var(--win98-inset-light);
  border-left-color: var(--win98-inset-light);
  border-right-color: var(--win98-inset-dark);
  border-bottom-color: var(--win98-inset-dark);
}

.ascii-art {
  font-family: monospace;
  white-space: pre;
  line-height: 1;
  text-align: center;
  margin: 10px 0;
  font-size: 12px;
}

.win98-section {
  margin: 15px 0;
}

.win98-section-title {
  margin-bottom: 10px;
  text-align: center;
  font-weight: bold;
}

.win98-inset-border {
  display: inline-block;
  padding: 5px 15px;
  border-bottom: 1px solid var(--win98-button-highlight);
  border-right: 1px solid var(--win98-button-highlight);
  border-top: 1px solid var(--win98-button-dark-shadow);
  border-left: 1px solid var(--win98-button-dark-shadow);
  font-weight: normal;
}

.win98-inset {
  border: 2px solid;
  border-top-color: var(--win98-inset-light);
  border-left-color: var(--win98-inset-light);
  border-right-color: var(--win98-inset-dark);
  border-bottom-color: var(--win98-inset-dark);
  padding: 10px;
  background-color: white;
}

.project-navigation, .music-navigation {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.project-item, .music-item {
  width: 400px;
  text-align: center;
}

.project-title {
  margin-bottom: 5px;
}

.project-description {
  margin-bottom: 5px;
}

.win98-link {
  color: blue;
  text-decoration: underline;
}

.music-image {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
  border: 1px solid var(--win98-button-shadow);
}

.contact-section {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.contact-button {
  padding: 5px 20px;
}

.footer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.duck-icon {
  font-size: 20px;
}

.nav-button {
  font-weight: bold;
  padding: 2px 8px;
}
</style>