<template>
  <section class="homepage">
    <!-- Template for page title. -->
    <div class="container">
      <p class="text">
        {{ $prismic.richTextAsPlain(text) }}
      </p>
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
      text: null,
    }
  },
  methods: {
    getContent () {
      //Query to get home content
      this.$prismic.client.getSingle('homepage')
        .then((document) => {
          if (document) {
            this.text = document.data.text;

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
