<template>
  <div id="app">
    <Header @toggleMenu="toggleMenu"/>
    <main>
      <nav class="main__nav" :class="{ active: isNavOpen }">
        <div class="nav__search">
          <input class="nav__search-input" placeholder="Type to search" v-model="searchKeywords" @keyup="search"/>
          <svg v-if="searchResultsVisible" class="nav__search-icon" tabindex="0" 
            @click="reset" 
            @keyup.enter="reset" 
            xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52"><path d="M26 0C11.664 0 0 11.663 0 26s11.664 26 26 26 26-11.663 26-26S40.336 0 26 0zm0 50C12.767 50 2 39.233 2 26S12.767 2 26 2s24 10.767 24 24-10.767 24-24 24z"/><path d="M35.707 16.293a.999.999 0 0 0-1.414 0L26 24.586l-8.293-8.293a.999.999 0 1 0-1.414 1.414L24.586 26l-8.293 8.293a.999.999 0 1 0 1.414 1.414L26 27.414l8.293 8.293a.997.997 0 0 0 1.414 0 .999.999 0 0 0 0-1.414L27.414 26l8.293-8.293a.999.999 0 0 0 0-1.414z"/></svg>
        </div>
        <vue-tree-navigation :items="tocItems" :defaultOpenLevel="1" />
      </nav>
      <router-view v-if="!searchResultsVisible"></router-view>
      <div class="container" v-else>
        <h1 class="results__heading" v-if="searchResults.length">{{searchResults.length}} results matching "{{searchKeywords}}"</h1>
        <h1 class="results__heading" v-else>No results matching "{{searchKeywords}}"</h1>

        <ul class="results__search">
          <li tabindex="0" v-for="(result, index) in searchResults" 
            @click="goTo(result.path)" 
            @keyup.enter="goTo(result.path)">
            <h2>{{ result.name }}</h2>
            <p>{{ result.text | truncate(300, '...') }}</p>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import ConfigManager from './services/configManager'
import Header from './components/Header'

export default {
  name: 'app',
  data () {
    return {
      isNavOpen: false,
      tocItems: ConfigManager.getPages(),
      content: [],
      searchResultsVisible: false,
      searchResults: null,
      searchKeywords: null
    }
  },
  components: {
    Header
  },
  created () {
    ConfigManager.getStatus()
      .then((data) => {
        if (data !== undefined) this.content = data
      })
      .catch((error) => {
        console.log('error', error)
      })
  },
  methods: {
    toggleMenu () {
      this.isNavOpen = !this.isNavOpen
    },
    search ($event) {
      if (this.searchKeywords) {
        this.searchResultsVisible = true
        this.searchResults = this.content.filter(result => {
          return (result.text || '').toLowerCase().indexOf(this.searchKeywords.toLowerCase()) > -1 ||
            (result.name || '').toLowerCase().indexOf(this.searchKeywords.toLowerCase()) > -1
        })
      } else {
        this.reset()
      }
    },
    goTo (path) {
      this.$router.push({ path })
      this.reset()
    },
    reset () {
      this.searchResultsVisible = false
      this.searchKeywords = ''
      this.searchResults = []
    }
  }
}
</script>

<style lang="scss">
  @import 'styles.scss';
</style>
