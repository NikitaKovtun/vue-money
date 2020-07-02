<template>
  <div id="app">
      <ATMDisplay @showError="showErrorPopup" @showThank="showThankPopup"/>
      <div class="popup-block" v-if="errorPopup || thankPopup">
        <ErrorPopup v-if="errorPopup"/>
        <ThankPopup v-if="thankPopup" :text="thankPopupText"/>
      </div>
  </div>
</template>

<script>
import ATMDisplay from './components/ATMDisplay.vue'
import ErrorPopup from './components/ErrorPopup.vue'
import ThankPopup from './components/ThankPopup.vue'

export default {
  name: 'App',
  components: {
    ATMDisplay,
    ErrorPopup,
    ThankPopup
  },
  data () {
    return {
      errorPopup: false,
      thankPopup: false,
      thankPopupText: ''
    }
  },
  methods: {
    showErrorPopup () {
      this.errorPopup = true
      setTimeout(() => { this.errorPopup = false }, 3000)
    },
    showThankPopup (text) {
      this.thankPopupText = text
      this.thankPopup = true
      setTimeout(() => { this.thankPopup = false }, 3000)
    }
  }
}
</script>

<style lang="sass">
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px
  .popup-block
    display: flex
    align-items: center
    justify-content: center
    position: fixed
    top: 0
    left: 0
    width: 100%
    height: 100%
    background-color: rgba(0, 0, 0, .8)
</style>
