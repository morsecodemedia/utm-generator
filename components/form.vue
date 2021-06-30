<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
    class="form"
  >
    <v-text-field
      v-model="website"
      :rules="websiteRules"
      label="Website URL*"
      hint="https://www.yourdomain.com/"
      persistent-hint
      required
    />

    <v-text-field
      v-model="campaignSource"
      label="Campaign Source"
      hint="referrer: google, facebook, newsletter"
      persistent-hint
    />

    <v-text-field
      v-model="campaignMedium"
      label="Campaign Medium"
      hint="marketing medium: cpc, banner, email, social"
      persistent-hint
    />

    <v-text-field
      v-model="campaignName"
      label="Campaign Name"
      hint="e.g. product, promo code, slogan"
      persistent-hint
    />

    <v-text-field
      v-model="campaignTerm"
      label="Campaign Term"
      hint="(optional) Identify the paid keywords"
      persistent-hint
    />

    <v-text-field
      v-model="campaignContent"
      label="Campaign Content"
      hint="(optional) use to differentiate ads"
      persistent-hint
    />

    <v-btn
      dark
      color="success"
      @click="validate"
    >
      Build URL
    </v-btn>

    <v-btn
      dark
      color="info"
      @click="reset"
    >
      Reset Form
    </v-btn>

    <v-text-field
      v-model="generatedURL"
      label="Your Generated URL"
    />

    <v-btn
      dark
      color="warning"
    >
      Copy URL
    </v-btn>
  </v-form>
</template>

<script>
export default {
  name: 'Form',
  data () {
    return {
      valid: true,
      website: '',
      websiteRules: [
        v => !!v || 'An URL is required'
      ],
      campaignSource: '',
      campaignMedium: '',
      campaignName: '',
      campaignTerm: '',
      campaignContent: '',
      hasParams: false,
      params: '',
      hash: '',
      hashStart: '',
      generatedURL: '',
      generatedLinkUrl: ''
    }
  },
  methods: {
    validate () {
      this.$refs.form.validate()
      if (this.valid) {
        this.buildUTM()
      }
    },
    reset () {
      this.$refs.form.reset()
      this.hash = ''
      this.hashStart = ''
      this.generatedURL = ''
    },
    buildUTM () {
      this.generatedURL = this.website
      this.generatedLinkUrl = this.website

      if (this.generatedURL.indexOf('#') > 0) {
        this.hash_start = this.generatedURL.indexOf('#')
        this.hash = this.website.substring(this.hash_start)
        this.generatedURL = this.website.substring(0, this.hash_start)
        this.generatedLinkUrl = this.website.substring(0, this.hash_start)
      }

      if (this.generatedURL.indexOf('https') !== 0) {
        this.generatedURL = 'https://' + this.generatedURL.trim()
        this.generatedLinkUrl = 'https://' + this.generatedURL.trim()
      }

      if (this.generatedURL.includes('/', 10) < 0 && this.generatedURL.includes('?') < 0) {
        this.generatedURL += '/'
      }
      this.campaignSource = encodeURIComponent(this.campaignSource.trim())

      if (this.campaignSource) {
        this.params += 'utm_source=' + this.campaignSource
        this.has_params = true
      }

      if (this.campaignMedium.trim() !== '') {
        this.params += this.has_params ? '&utm_medium=' + encodeURIComponent(this.campaignMedium.trim()) : 'utm_medium=' + encodeURIComponent(this.campaignMedium.trim())
        this.has_params = true
      }

      if (this.campaignName.trim() !== '') {
        this.params += this.has_params ? '&utm_campaign=' + encodeURIComponent(this.campaignName.trim()) : 'utm_campaign=' + encodeURIComponent(this.campaignName.trim())
        this.has_params = true
      }

      if (this.campaignTerm.trim() !== '') {
        this.params += this.has_params ? '&utm_term=' + encodeURIComponent(this.campaignTerm.trim()) : 'utm_term=' + encodeURIComponent(this.campaignTerm.trim())
        this.has_params = true
      }

      if (this.campaignContent.trim() !== '') {
        this.params += this.has_params ? '&utm_content=' + encodeURIComponent(this.campaignContent.trim()) : 'utm_content=' + encodeURIComponent(this.campaignContent.trim())
        this.has_params = true
      }

      if (this.has_params) {
        if (this.generatedURL.indexOf('?') > 0) {
          this.generatedURL += '&' + this.params
        } else {
          this.generatedURL += '?' + this.params
        }
      }
      this.generatedURL += this.hash
    }
  }
}
</script>

<style>
.form {
  margin: 0 auto;
  max-width: 1000px;
  padding: 0 20px;
}
.v-input,
.v-btn {
  margin-bottom: 20px;
}
</style>
