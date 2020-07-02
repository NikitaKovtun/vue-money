<template>
  <div class="bill-component">
    <div class="bill-component-img">
      <img :src="require(`@/assets/${img}`)" alt="">
    </div>
    <div class="bill-component-text">
      <div class="bill-component-text-name">
        <span>{{name}} грн. - {{count}} шт.</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BillComponent',
  props: {
    count: {
      type: Number,
      default: 0
    },
    img: {
      type: String,
      default: ''
    },
    name: {
      type: Number,
      default: 0
    }
  }
}
</script>

<style lang="sass" scoped>
.bill-component
  width: 250px
  margin: 15px
  &-img
    width: 100%
    img
      width: 100%
      height: 150px
      object-fit: contain
      display: block
</style>
