<template>
  <table>
    <tr>
      <td>
        <div>
          {{ dividend }}
        </div>
        <div>
          <division-item
            v-for="(remainder, index) in division"
            :key="index"
            :division="remainder"
            :position="index"
          />
        </div>
      </td>
      <td class="divider-block">
        <div class="divider-text">
          {{ divider }}
        </div>
        <div class="divided-text">
          {{ divided }}
        </div>
      </td>
    </tr>
  </table>
</template>

<script>
import DivisionItem from '@/components/DivisionItem'

export default {
  name: 'DivisionTable',

  components: {
    DivisionItem
  },

  props: {
    dividend: {
      type: Number,
      required: true
    },

    divider: {
      type: Number,
      required: true
    },

    divided: {
      type: Number,
      required: true
    },

    division: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>
table {
  text-align: initial;
}

.divider-block {
  display: flex;
  flex-direction: column;
  border-left: 1px solid #000;
}
.divider-text {
  border-bottom: 1px solid #000;
  padding: 0 20px 5px 20px;
}
.divided-text {
  padding: 5px 20px 0 20px;
}
</style>
