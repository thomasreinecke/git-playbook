<template>
  <section class="banner">
    <h1 class="banner__title heading-1">{{ title }}</h1>
    <p class="banner__text" v-html="text"></p>
  </section>
</template>

<script>

export default {
  name: 'banner',
  props: ['title', 'text'],
  data () {
    return {
    }
  }
}
</script>

<style>

</style>
