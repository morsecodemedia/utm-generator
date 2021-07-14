<template>
  <Form />
</template>

<script>
import Form from '~/components/form.vue'
export default {
  name: 'Homepage',
  components: {
    Form
  },
  head () {
    return {
      title: 'UTM Generator | a morsecodemedia tool',
      meta: [
        { hid: 'ogtitle', property: 'og:title', content: 'UTM Generator | a morsecodemedia tool' },
        { hid: 'twtitle', name: 'twitter:title', content: 'UTM Generator | a morsecodemedia tool' },
        { hid: 'googlename', itemprop: 'name', content: 'UTM Generator | a morsecodemedia tool' },
        { hid: 'description', name: 'description', content: 'A simple form to help users generate UTM codes for URLs.' },
        { hid: 'ogdescription', property: 'og:description', content: 'A simple form to help users generate UTM codes for URLs.' },
        { hid: 'twdescription', name: 'twitter:description', content: 'A simple form to help users generate UTM codes for URLs.' },
        { hid: 'googledescription', itemprop: 'description', content: 'A simple form to help users generate UTM codes for URLs.' },
        { hid: 'ogurl', property: 'og:url', content: 'https://utm-generator.morsecodemedia.com' + this.$route.path },
        { hid: 'twsite', name: 'twitter:site', content: 'https://utm-generator.morsecodemedia.com' + this.$route.path }
      ],
      link: [
        { hid: 'canonical', rel: 'canonical', href: 'https://utm-generator.morsecodemedia.com' + this.$route.path }
      ]
    }
  }
}
</script>

<style>

</style>
