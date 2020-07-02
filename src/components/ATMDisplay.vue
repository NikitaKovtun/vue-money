<template>
  <div class="atm-display">
    <div v-html="thankText"></div>
    <div class="input-block">
      <div class="input-block-title">
        Введите нужную сумму
      </div>
      <div class="input-block-input">
        <input type="number" min="0" v-model="userAmount" ref="userAmount">
        <button @click="checkUserAmount" :disabled="allMoney === 0">
          выдать
        </button>
      </div>
    </div>
    <div class="bill-list">
      <BillComponent v-for="(bill, key) of billsList"
                     :count="bill.count"
                     :img="bill.img"
                     :name="bill.value"
                     :key="key"/>
    </div>
  </div>
</template>

<script>
import BillComponent from './BillComponent.vue'

export default {
  name: 'ATMDisplay',
  components: {
    BillComponent
  },
  data () {
    return {
      billsList: [
        {
          value: 500,
          count: 20,
          img: '500_uah.jpg'
        },
        {
          value: 200,
          count: 50,
          img: '200_uah.jpeg'
        },
        {
          value: 100,
          count: 20,
          img: '100_uah.jpg'
        },
        {
          value: 50,
          count: 20,
          img: '50_uah.jpg'
        },
        {
          value: 20,
          count: 20,
          img: '20_uah.jpg'
        },
        {
          value: 10,
          count: 20,
          img: '10_uah.jpg'
        }
      ],
      calcBillList: [],
      userAmount: null,
      calcAmount: 0,
      billsAmount: {
        500: 0,
        200: 0,
        100: 0,
        50: 0,
        20: 0,
        10: 0
      },
      thankText: ''
    }
  },
  computed: {
    allMoney: function () {
      return this.billsList.reduce((prev, bill) => {
        return prev + (bill.value * bill.count)
      }, 0)
    }
  },
  methods: {
    checkUserAmount () {
      if (!this.userAmount || this.userAmount < 0 || this.userAmount > this.allMoney || this.userAmount % 10 !== 0) {
        this.userAmount = null
        this.$emit('showError')
      } else {
        this.calcAmount = this.userAmount
        this.calcBillList = this.billsList.concat()
        this.splitAmountIntoBills(this.calcBillList)
      }
    },
    countBills (bill) {
      const possibleBillCount = Math.trunc(this.calcAmount / bill.value)
      if (possibleBillCount > bill.count) {
        this.billsAmount[bill.value] = bill.count
        this.calcAmount = this.calcAmount - bill.value * bill.count
      } else {
        this.billsAmount[bill.value] = possibleBillCount
        this.calcAmount = this.calcAmount % bill.value
      }
    },
    updateBillsCount () {
      for (const bill of this.billsList) {
        if (this.billsAmount[bill.value]) {
          this.thankText += `${this.billsAmount[bill.value]}шт. номиналом ${bill.value} <br>`
          bill.count = bill.count - this.billsAmount[bill.value]
        }
      }
      this.thankText += `Общая полученная сумма: ${this.userAmount}грн.`
      this.$emit('showThank', this.thankText)
      this.userAmount = null
      this.thankText = ''
      this.billsAmount = {
        500: 0,
        200: 0,
        100: 0,
        50: 0,
        20: 0,
        10: 0
      }
    },
    splitAmountIntoBills (billList) {
      for (const bill of billList) {
        if (this.calcAmount) {
          this.countBills(bill)
        } else {
          break
        }
      }
      if (this.calcAmount > 0 && billList.length > 0) {
        this.calcAmount = this.userAmount
        this.billsAmount = {
          500: 0,
          200: 0,
          100: 0,
          50: 0,
          20: 0,
          10: 0
        }
        billList.splice(0, 1)
        this.splitAmountIntoBills(billList)
      } else if (this.calcAmount !== 0 && billList.length === 0) {
        this.$emit('showError')
        this.userAmount = null
      } else {
        this.updateBillsCount()
      }
    }
  }
}
</script>

<style lang="sass" scoped>
  .atm-display
    padding: 0 30px
    .input-block
      display: flex
      flex-direction: column
      align-items: center
      justify-content: center
      &-title
        font-size: 18px
        margin-bottom: 20px
      &-input
        display: flex
        align-items: center
        input
          margin-right: 15px
          font-size: 16px
          padding: 10px
          border: 1px solid #cccccc
          border-radius: 5px
          box-shadow: none
          outline: none
        button
          padding: 10px
          outline: none
          border: none
          border-radius: 5px
          box-shadow: none
          background-color: #5879a4
          color: #ffffff
          cursor: pointer
          &:hover
            background-color: #2c3e50
    .bill-list
      display: flex
      justify-content: center
      align-items: center
      flex-wrap: wrap
</style>
