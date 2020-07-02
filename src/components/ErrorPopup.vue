<template>
  <div class="errorPopup">
    <span>К сожалению эту сумму выдать не возможно</span>
  </div>
</template>

<script>
export default {
  name: 'ErrorPopup'
}
</script>

<style lang="sass" scoped>
.errorPopup
  min-width: 250px
  max-width: 50%
  background-color: #c68a8a
  color: #ffffff
  border-radius: 10px
  padding: 20px
  position: absolute
  top: 50%
  left: 50%
  transform: translate(-50%, -50%)
</style>
