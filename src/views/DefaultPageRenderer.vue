<template>
  <div class="container" @scroll="handleScroll">
    <section class="banner" 
      :class="{ active: isActive }" 
      v-if="pageConfig && !pageConfig.tiles"
      :data-name="pageConfig.name">
      
      <h2 class="banner__title heading-1">{{ pageConfig.name }}</h2>
      <h4 class="banner__title heading-4">{{ pageConfig.description }}</h4>
      <p class="banner__text banner__text--timestamp">{{ lastModified }}</p>
    </section>

    <vue-markdown class="content" :source="markdown"></vue-markdown>

    <ul class="cards" v-if="pageConfig.tiles">
      <li class="card" @click="switchPage(tile)" v-for="tile in pageConfig.tiles" v-bind:style="{ backgroundColor: tile.bgColor }">
        <font-awesome-icon class="card__icon" size="2x" :icon="tile.icon"/>
        <h4 class="card__title">{{tile.name}}</h4>
        <p class="card__text">{{tile.description}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import ConfigManager from '../services/configManager'
import VueMarkdown from 'vue-markdown'
import axios from 'axios'

export default {
  components: {
    VueMarkdown
  },
  data () {
    return {
      markdown: '',
      lastModified: '',
      pageConfig: {},
      tocItems: ConfigManager.getPages(),
      isActive: false,
      container: null,
      initialScroll: false
    }
  },
  mounted () {
    this.initialize(this.$router.currentRoute.path)
    setTimeout(() => {
      this.scrollTo(this.$route.hash)
    }, 150)

    this.container = document.querySelector('main > .container')
    if (this.container) {
      this.container.addEventListener('scroll', (event) => {
      })
    }
    console.log('this.container', this.container)
    setTimeout(() => {
      const iframes = document.querySelectorAll('iframe')
      console.log('iframes', iframes)
      for (let iframe of iframes) {
        iframe.addEventListener('load', () => {
          console.log('iframe LOADED')
        })
        console.log('iframe', iframe)
      }
    }, 150)
  },
  watch: {
    '$route' (to, from) {
      if (to.path !== from.path) {
        this.pageConfig = to.meta
        this.initialize()
      } else {
        setTimeout(() => {
          if (to.hash) {
            this.scrollTo(to.hash)
            console.log('to.hash', to.hash)
            if (to.hash.toLowerCase() === '#toc') {
              console.log('this.$route', this.$route)
              this.$router.push({ path: to.path })
              // this.$route.push
            }
          }
        }, 10)
      }
    }
  },
  methods: {
    onLoadIFrame (event) {
      console.log('onload iframe', event)
    },
    /**
     * initialize - called whenever the DefaultPageRenderer needs to re-initialize to render a specific page
     */
    initialize (path) {
      if (!this.pageConfig.name) {
        let currentPath = this.$router.currentRoute.path
        this.pageConfig = ConfigManager.getMetaById(currentPath)
      }

      // load the markdown ressource from its static ressources
      this.markdown = ''
      if (this.pageConfig.markdown !== undefined) {
        const pathName = window.location.pathname
        const path = `${pathName.substring(0, pathName.length - 1)}${this.pageConfig.markdown}`
        var config = {headers: {'Cache-Control': 'no-cache'}}
        axios.get(path, config).then(response => {
          this.lastModified = response.headers['last-modified']
          console.log(this.lastModified)
          this.markdown = response.data
        })
      }
    },
    switchPage (tile) {
      this.$router.push(tile.path)
    },
    handleScroll (event) {
      if (event.target.scrollTop >= 155) {
        this.isActive = true
      } else {
        this.isActive = false
      }
    },
    scrollTo (hash) {
      if (hash) {
        let target = document.querySelector(`.content ${hash.toLowerCase()}`)
        if (target) {
          target.scrollIntoView(true)
          this.initialScroll = true
        }
      } else {
        document.querySelector(`main > .container`).scrollTo(0, 0)
        this.initialScroll = true
      }
    }
  }
}
</script>