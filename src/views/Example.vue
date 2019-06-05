<template>
  <section class="container">
    <h2>
        {{ $prismic.richTextAsPlain(title) }}
    </h2>
    <section class="page">
      <div class="content">
        <!-- Slice section template -->
        <slices-block :slices="slices"/>
      </div>
      <div class="table-of-contents">
        <h3>Table of Contents</h3>
        <ul>
          <il class="headingSection">
            <a href="#">
              {{ $prismic.richTextAsPlain(title) }}
            </a>
          </il>
          <section v-for="(slice, index) in slices" :key="'slice-' + index">
            <template v-if="slice.slice_type === 'text_section'">
              <section v-for="(element, index) in slice.primary.rich_text" :key="'element-' + index">
                <template v-if="element.type === 'heading2'">
                  <li class="headingSection">
                    <a :href="'#' + element.text.replace(/\W+/g, '-').toLowerCase()">
                      {{ element.text }}
                    </a>
                  </li>
                </template>
                <template v-if="element.type === 'heading3'">
                  <li class="headingParagraph">
                    <a :href="'#' + element.text.replace(/\W+/g, '-').toLowerCase()">
                      {{ element.text }}
                    </a>
                  </li>
                </template>
              </section>
            </template>
          </section>
        </ul>
      </div>
    </section>
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

<style>
.page {
  display: flex;  
  flex-flow: row wrap;
}
.content {
  width: 70%;
}
.table-of-contents {
  position: fixed;
  width: 25%;
  right: 5%;
}
.table-of-contents ul li.headingParagraph {
  padding-left: 15px;
}
</style>
