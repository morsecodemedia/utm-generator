<template>
  <div class="form">
    <v-form
      ref="form"
    >
      <v-text-field
        label="Website URL*"
        hint="https://www.yourdomain.com/"
        persistent-hint
      ></v-text-field>

      <v-text-field
        label="Campaign Source"
        hint="referrer: google, facebook, newsletter"
        persistent-hint
      ></v-text-field>

      <v-text-field
        label="Campaign Medium"
        hint="marketing medium: cpc, banner, email, social"
        persistent-hint
      ></v-text-field>

      <v-text-field
        label="Campaign Name"
        hint="e.g. product, promo code, slogan"
        persistent-hint
      ></v-text-field>

      <v-text-field
        label="Campaign Term"
        hint="(optional) Identify the paid keywords"
        persistent-hint
      ></v-text-field>

      <v-text-field
        label="Campaign Content"
        hint="(optional) use to differentiate ads"
        persistent-hint
      ></v-text-field>

      <v-btn
        :disabled="!valid"
        color="success"
        class="mr-4"
      >
        Build URL
      </v-btn>

      <v-btn
        :disabled="!valid"
        color="error"
        class="mr-4"
      >
        Reset Form
      </v-btn>
    </v-form>

    <v-text-field
      label="Your Generated URL"
    ></v-text-field>
    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
    >
      Copy URL
    </v-btn>
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
</style>
