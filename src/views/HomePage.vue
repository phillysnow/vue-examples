<template>
  <section class="homepage">
    <!-- Template for page title. -->
    <div class="container">
      <h2 class="title">
        {{ $prismic.richTextAsPlain(title) }}
      </h2>
    </div>
    <examples-grid/>
  </section>
</template>

<script>
// imports for all components
import ExamplesGrid from '../components/ExamplesGrid.vue'

export default {
  name: 'homepage',
  components: {
    ExamplesGrid,
  },
  data () {
    return {
      title: null,
    }
  },
  methods: {
    getContent () {
      //Query to get home content
      this.$prismic.client.getSingle('homepage')
        .then((document) => {
          if (document) {
            this.title = document.data.title;

          } else {
            //returns error page
            this.$router.push({ name: 'not-found' })
          }
        })
    },
  },
  created () {
    this.getContent()
  }
}
</script>

<style>
</style>
