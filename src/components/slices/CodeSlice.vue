<template>
    <section class="block">
        <code>
            <prism :language="slice.primary.lang">{{ $prismic.richTextAsPlain(slice.primary.code) }}</prism>
        </code>
    </section>
</template>

<script>
import Prism from 'vue-prism-component'

export default {
    props: ['slice'],
    name: 'code-slice',
    components: {
    Prism
  }
}
</script>

<style>
.block {
    margin: 50px 0 50px 0;
}
</style>

