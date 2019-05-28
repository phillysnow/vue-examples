<template>
  <section class="page">
    <div class="container">
      <h2>
        {{ $prismic.richTextAsPlain(title) }}
      </h2>
      <!-- Slice section template -->
      <slices-block :slices="slices"/>
    </div>
  </section>
</template>

<script>
// imports for all components
import SlicesBlock from '../components/SlicesBlock.vue'

export default {
  name: 'example',
  components: {
    SlicesBlock,
  },
  data () {
    return {
      title: null,
      slices: []
    }
  },
  methods: {
    getContent (uid) {
      //Query to get post content
      this.$prismic.client.getByUID('example', uid)
        .then((document) => {
          if (document) {
            this.title = document.data.title;
            
            //Set slices as variable
            this.slices = document.data.body
          } 
          else {
            //returns error page
            this.$router.push({ name: 'not-found' })
          }
        })
    }
  },
  created () {
    this.getContent(this.$route.params.uid)
  },
  beforeRouteUpdate (to, from, next) {
    this.getContent(to.params.uid)
    next()
  }
}
</script>