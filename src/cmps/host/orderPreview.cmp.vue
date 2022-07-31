
<template class="order-preview">
  <div class="card">
    <span>{{ this.order.createAt }}</span>
    <span>{{ this.currOrder.checkIn }}</span>
    <span> {{ this.currOrder.chackOut }}</span>
    <span> {{ this.order.guestsDetails.guestName }}</span>
    <span> {{ this.order.totalPrice }}</span>
    <el-select @change="changeStatus" v-model="this.currOrder.status">
      <el-option v-for="(status, idx) in this.statusArry" :value="status" :key="idx">
        {{ status }}
      </el-option>
    </el-select>
  </div>
</template>

<script>


export default {
  props: {
    order: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      currOrder: {
        _id: this.order._id,
        name: this.order.stay.name,
        checkIn: new Date(this.order.checkin).toLocaleDateString("en", { year: "numeric", month: "short", day: "numeric" }),
        chackOut: new Date(this.order.checkout).toLocaleDateString("en", { year: "numeric", month: "short", day: "numeric" }),
        guest: this.order.guestsDetails.guestName,
        totalPrice: this.order.totalPrice,
        status: this.order.status,
      },
      statusArry: [
        'pending',
        'Confirm',
        'Rejected',
      ],

    }
  },
  created() {
    // console.log(this.currOrder)
  },
  methods: {
    changeStatus() {
      this.$store.dispatch({
        type: 'changeOrderStatus',
        order: this.currOrder
      })
    },




  },
}

</script>

