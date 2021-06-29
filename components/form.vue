<template>
  <div class="form">
    <form>
      <label for="url">Website URL*</label>
      <input
        v-model="url"
        type="text"
        placeholder="https://www.yourdomain.com/"
        aria-placeholder="https://www.yourdomain.com/"
      >

      <label for="source">Campaign Source</label>
      <input
        v-model="source"
        type="text"
        placeholder="referrer: google, facebook, newsletter"
        aria-placeholder="referrer: google, facebook, newsletter"
      >

      <label for="medium">Campaign Medium</label>
      <input
        v-model="medium"
        type="text"
        placeholder="marketing medium: cpc, banner, email, social"
        aria-placeholder="marketing medium: cpc, banner, email, social"
      >

      <label for="campaign">Campaign Name</label>
      <input
        v-model="campaign"
        type="text"
        placeholder="e.g. product, promo code, slogan"
        aria-placeholder="e.g. product, promo code, slogan"
      >

      <label for="term">Campaign Term</label>
      <input
        v-model="term"
        type="text"
        placeholder="(optional) Identify the paid keywords"
        aria-placeholder="(optional) Identify the paid keywords"
      >

      <label for="content">Campaign Content</label>
      <input
        v-model="content"
        type="text"
        placeholder="(optional) use to differentiate ads"
        aria-placeholder="(optional) use to differentiate ads"
      >

      <button>Build URL</button>

      <button>Clear Form</button>
    </form>

    <label for="generated-url">Your Generated URL:</label>
    <input
      v-model="result"
      type="text"
    >
    <button>Copy URL</button>
  </div>
</template>

<script>
export default {
  name: 'Form',
  data () {
    return {
      url: '',
      source: '',
      medium: '',
      campaign: '',
      term: '',
      content: '',
      anchor: '', // will this be needed?
      result: '' // final url built
    }
  }
}
</script>

<style>
.form {
  margin: 0 auto;
  max-width: 1000px;
}
form {
  display: flex;
  flex-direction: column;
  margin-bottom: 35px;
}

button {
  display: inline-block;
}

input {
  margin-bottom: 15px;
}
</style>
