<template>
  <div class="thankPopup">
   <div class="thankPopup-title">
     Спасибо!
   </div>
    <div class="thankPopup-text-block">
      <span>Вы получили следующие банкноты:</span>
      <br>
      <span v-html="text">
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ThankPopup',
  props: {
    text: {
      type: String,
      default: ''
    }
  }
}
</script>

<style lang="sass" scoped>
.thankPopup
  min-width: 250px
  max-width: 50%
  background-color: #5abc53
  color: #ffffff
  border-radius: 10px
  padding: 20px
  position: absolute
  top: 50%
  left: 50%
  transform: translate(-50%, -50%)
</style>
