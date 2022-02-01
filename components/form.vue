<template>
  <v-app>
    <v-card
      class="mx-auto"
      flat
      max-width="1000"
    >
      <v-container fluid>
        <h1>UTM Generator</h1>
        <v-form
          ref="form"
          v-model="valid"
          lazy-validation
          class="form"
        >
          <p>Fill in the information in the URL builder below and click the Build URL button.</p>
          <p><strong>Note:</strong> UTM parameters are <em>case sensitive</em> and you should be consistent when tagging your links.</p>
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
            :messages="[caseSensitiveSource]"
            label="Campaign Source"
            hint="referrer: google, facebook, newsletter"
            persistent-hint
          />

          <v-text-field
            v-model="campaignMedium"
            :messages="[caseSensitiveMedium]"
            label="Campaign Medium"
            hint="marketing medium: cpc, banner, email, social"
            persistent-hint
          />

          <v-text-field
            v-model="campaignName"
            :messages="[caseSensitiveCampaign]"
            label="Campaign Name"
            hint="e.g. product, promo code, slogan"
            persistent-hint
          />

          <v-text-field
            v-model="campaignTerm"
            :messages="[caseSensitiveTerm]"
            label="Campaign Term"
            hint="(optional) use to identify the paid keywords"
            persistent-hint
          />

          <v-text-field
            v-model="campaignContent"
            :messages="[caseSensitiveContent]"
            label="Campaign Content"
            hint="(optional) use to differentiate ads"
            persistent-hint
          />

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
            readonly
          />

          <v-btn
            v-clipboard:copy="generatedURL"
            dark
            color="warning"
          >
            Copy URL
          </v-btn>
        </v-form>
      </v-container>
    </v-card>
  </v-app>
</template>

<script>
import Vue from 'vue'
import VueClipboard from 'vue-clipboard2'

Vue.use(VueClipboard)

export default {
  name: 'Form',
  data () {
    return {
      valid: true,
      website: 'https://utm-generator.morsecodemedia.com/',
      websiteRules: [
        v => !!v || 'An URL is required'
      ],
      campaignSource: '',
      campaignMedium: '',
      campaignName: '',
      campaignTerm: '',
      campaignContent: '',
      allowPlusSign: '/%2B/i'
    }
  },
  computed: {
    generatedURL () {
      let accumulator
      let params = ''
      let hash = ''

      if (this.website.indexOf('#') > 0) {
        const hashStart = this.website.indexOf('#')
        hash = this.website.substring(hashStart)
        accumulator = this.website.substring(0, hashStart)
      } else {
        accumulator = this.website
      }

      if (accumulator.indexOf('https') !== 0) {
        accumulator = 'https://' + accumulator.trim()
      }

      if (accumulator.includes('/', 10) < 0 && accumulator.includes('?') < 0) {
        accumulator += '/'
      }
      // this.campaignSource = encodeURIComponent(this.campaignSource.trim())

      if (this.campaignSource) {
        params += 'utm_source=' + encodeURIComponent(this.campaignSource.trim())
      }

      if (this.campaignMedium.trim() !== '') {
        if (params.length) {
          params += '&'
        }
        params += 'utm_medium=' + encodeURIComponent(this.campaignMedium.trim())
      }

      if (this.campaignName.trim() !== '') {
        if (params.length) {
          params += '&'
        }
        params += 'utm_campaign=' + encodeURIComponent(this.campaignName.trim())
      }

      if (this.campaignTerm.trim() !== '') {
        if (params.length) {
          params += '&'
        }
        params += 'utm_term=' + encodeURIComponent(this.campaignTerm.trim())
      }

      if (this.campaignContent.trim() !== '') {
        if (params.length) {
          params += '&'
        }
        params += 'utm_content=' + encodeURIComponent(this.campaignContent.trim())
      }

      if (params.length) {
        if (accumulator.indexOf('?') > 0) {
          accumulator += '&' + params
        } else {
          accumulator += '?' + params
        }
      }

      accumulator += hash

      return accumulator
    }
  },
  mounted () {
    if (process.browser) {
      if (this.getParameterByName('utm_source')) {
        this.campaignSource = this.getParameterByName('utm_source')
      }
      if (this.getParameterByName('utm_medium')) {
        this.campaignMedium = this.getParameterByName('utm_medium')
      }
      if (this.getParameterByName('utm_campaign')) {
        this.campaignName = this.getParameterByName('utm_campaign')
      }
      if (this.getParameterByName('utm_term')) {
        this.campaignTerm = this.getParameterByName('utm_term')
      }
      if (this.getParameterByName('utm_content')) {
        this.campaignContent = this.getParameterByName('utm_content')
      }
    }
  },
  methods: {
    reset () {
      this.$refs.form.reset()
      this.website = ''
      this.campaignSource = ''
      this.campaignMedium = ''
      this.campaignName = ''
      this.campaignTerm = ''
      this.campaignContent = ''
    },
    getParameterByName (name, url) {
      if (process.browser) {
        try {
          if (!url) {
            url = document.location.href
          }
          const urlObj = new URL(url)
          return urlObj.searchParams.get(name)
        } catch (e) {
          console.log(e)
          return ''
        }
      }
    },
    caseSensitiveSource (value) {
      if (value.length > 0) {
        if (value === value.toLowerCase() || value === value.toUpperCase()) {
          return 'referrer: google, facebook, newsletter'
        } else {
          return 'Reminder: UTM parameters are case sensitive.'
        }
      }
      return 'referrer: google, facebook, newsletter'
    },
    caseSensitiveMedium (value) {
      if (value.length > 0) {
        if (value === value.toLowerCase() || value === value.toUpperCase()) {
          return 'marketing medium: cpc, banner, email, social'
        } else {
          return 'Reminder: UTM parameters are case sensitive.'
        }
      }
      return 'marketing medium: cpc, banner, email, social'
    },
    caseSensitiveCampaign (value) {
      if (value.length > 0) {
        if (value === value.toLowerCase() || value === value.toUpperCase()) {
          return 'e.g. product, promo code, slogan'
        } else {
          return 'Reminder: UTM parameters are case sensitive.'
        }
      }
      return 'e.g. product, promo code, slogan'
    },
    caseSensitiveTerm (value) {
      if (value.length > 0) {
        if (value === value.toLowerCase() || value === value.toUpperCase()) {
          return '(optional) use to identify the paid keywords'
        } else {
          return 'Reminder: UTM parameters are case sensitive.'
        }
      }
      return '(optional) use to identify the paid keywords'
    },
    caseSensitiveContent (value) {
      if (value.length > 0) {
        if (value === value.toLowerCase() || value === value.toUpperCase()) {
          return '(optional) use to differentiate ads'
        } else {
          return 'Reminder: UTM parameters are case sensitive.'
        }
      }
      return '(optional) use to differentiate ads'
    }
  }
}
</script>

<style>
.v-input,
.v-btn {
  margin-bottom: 20px;
}
</style>
