<template>
  <tr>
    <div>
      <div>
        <div v-if="position !== 0" class="sub-block">
          <span
            v-for="(count, index) in division.countSpace"
            :key="index"
            class="hide"
          >
            0
          </span>
          {{ division.number }}
        </div>
      </div>
      <div>
        <div class="division-bottom-line sub-block">
          <span
            v-for="(count, index) in division.countSpace + differenceLength"
            :key="index"
            class="hide"
          >
            0
          </span>
          {{ division.sum }}
        </div>
      </div>
    </div>
  </tr>
</template>

<script>
export default {
  name: 'DivisionItem',

  props: {
    division: {
      type: Object,
      required: true
    },

    position: {
      type: Number,
      require: true
    }
  },

  computed: {
    /**
     * Check if top number longer then lower number
     */
    differenceLength() {
      const sum = this.division.sum.toString().length

      const num = this.division.number.toString().length

      return num > sum ? num - sum : 0
    }
  }
}
</script>

<style scoped>
.sub-block {
  display: flex;
}

.division-bottom-line {
  border-bottom: 1px solid #000;
}
.hide {
  opacity: 0;
}
</style>
