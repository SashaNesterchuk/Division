<template>
  <div id="app">
    <div>
      <h2>
        Divide the numbers
      </h2>
      <input type="number" v-model="dividend" placeholder="30" />
      /
      <input type="number" v-model="divider" placeholder="3" />
    </div>
    <div>
      <div v-if="checkNumbers">
        {{ checkNumbers }}
      </div>
      <div v-if="divison.length" class="division-table-block">
        <division-table
          :dividend="Number(divindedWithoutDot)"
          :divider="Number(dividerWithoutDot)"
          :divided="Number(divison[0])"
          :division="divison[1]"
        />
      </div>
    </div>
  </div>
</template>

<script>
const COUNT_AFTER_DOT = 5

import DivisionTable from '@/components/DivisionTable'
export default {
  name: 'App',

  components: {
    DivisionTable
  },

  data() {
    return {
      dividend: '',
      divider: '',
      numbers: []
    }
  },

  computed: {
    checkNumbers() {
      return !Number(this.dividend) || !Number(this.divider)
        ? 'Incorrect numbers'
        : ''
    },

    isDividendFloat() {
      return this.dividend % 1 !== 0
    },

    isDividerFloat() {
      return this.divider % 1 !== 0
    },

    dividerWithoutDot() {
      return this.getNumberWithoutDot(
        this.divider,
        this.countAfterDot(this.divider),
        this.countAfterDot(this.dividend)
      )
    },

    divindedWithoutDot() {
      return this.getNumberWithoutDot(
        this.dividend,
        this.countAfterDot(this.dividend),
        this.countAfterDot(this.divider)
      )
    },

    isNumbersFloat() {
      return this.isDividendFloat || this.isDividerFloat
    },

    divison() {
      return this.mainDivision()
    }
  },

  methods: {
    mainDivision() {
      if (this.checkNumbers) {
        return []
      }

      this.notification = ''

      let numbers = this.divindedWithoutDot.toString().split('')

      return this.recuSubDivision(numbers, numbers[0])
    },

    countAfterDot(number) {
      let count = number.toString().indexOf('.')

      if (count === -1) return 0

      return Number(number.toString().length - (count + 1))
    },

    recuSubDivision(
      numbers,
      subNumber,
      index = 0,
      result = 0,
      numbersInfo = [],
      countSpace = 0
    ) {
      /**
       * Return if there is no residue
       */
      if (index + 1 >= numbers.length && subNumber <= 0)
        return [result, numbersInfo]

      index = this.getCurrentIndex(numbers, index, subNumber)

      /**
       * Get sub number for division
       */
      subNumber = this.getCurrentNumber(numbers, index, subNumber)

      let subDivision = Math.floor(subNumber / this.dividerWithoutDot)

      /**
       * Info for table
       */
      numbersInfo.push({
        countSpace: countSpace,
        number: subNumber,
        sum: Math.floor(subDivision * this.dividerWithoutDot)
      })

      /**
       * Checks the loop passed by the number of zeros
       */
      if (
        index >= numbers.length &&
        index - numbers.length + 1 === COUNT_AFTER_DOT
      ) {
        return [result.toString() + subDivision, numbersInfo]
      }

      /**
       * Add dot to the end
       */
      if (index === numbers.length) {
        result = result.toString() + '.' + subDivision
      } else {
        result = result.toString() + subDivision
      }

      return this.recuSubDivision(
        numbers,
        Math.floor(subNumber - subDivision * this.dividerWithoutDot),
        index,
        result,
        numbersInfo,
        countSpace + subNumber.toString().length - 1
      )
    },

    getCurrentNumber(numbers, index, subNumber) {
      if (index >= numbers.length) {
        return subNumber + '0'
      }

      if (subNumber < this.dividerWithoutDot) {
        return subNumber + numbers[index]
      }

      return subNumber
    },

    getCurrentIndex(numbers, index, subNumber) {
      if (index === numbers.length || subNumber < this.dividerWithoutDot) {
        return index + 1
      }

      return index
    },

    /**
     * Get number instead of dots zeros
     */
    getNumberWithoutDot(number, countAfterDot, countAfterDot2) {
      if (countAfterDot < countAfterDot2) {
        let diff = countAfterDot2 - countAfterDot

        number = number.toString()

        for (let i = 0; i < diff; i++) {
          number += '0'
        }

        return Number(number.replace('.', ''))
      }

      return Number(number.replace('.', ''))
    }
  }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type='number'] {
  -moz-appearance: textfield;
}

.division-table-block {
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
</style>
