<template >
    <div class="reservation-details flex reservation-layout ">
        <div class="flex reservation-header">
            <h2>
                ${{ stay.price }}
                <span>night</span>
            </h2>
            <div>
                <span> <i class=" fa fa-star"></i> {{ (stay.reviewScores.rating / 20).toFixed(2) }} <span
                        class="dot"></span></span>
                <span>
                    {{ stay.reviews.length }} reviews
                </span>
            </div>
        </div>
        <div class="option-container flex">
            <div @submit.stop class="date-container">
                <div class="checkin clickable">
                    <span class="top" >CHECKIN</span>
                    <span v-if="!date.length" class="bottom">Add dates</span>
                    <span v-else class="bottom">{{
                            date[0].getDate()
                    }}/{{ date[0].getMonth() + 1 }}/{{ date[0].getYear() - 100 }}</span>
                </div>
                <div class="checkout clickable ">
                    <span class="top" style="padding-bottom: 1px;">CHECKOUT</span>
                    <span v-if="!date.length" class="bottom">Add dates</span>
                    <span v-else class="bottom">{{ date[1].getDate() }}/{{ date[1].getMonth() + 1 }}/{{
                            date[1].getYear() -
                            100
                    }}</span>
                </div>
                <el-date-picker v-model="date" @change="changeDate" start-placeholder="Check in "
                    end-placeholder="Check out " type="daterange" range-separator="|"
                    style=">* border: none; height: 65px;" />
                <!-- </div> -->
            </div>
            <div class="dropdown">
                <button @click="toggle" class="dropbtn">
                    <h5>GUESTS</h5>
                    <span v-if="adultAmount <= 1">{{ adultAmount }} guest</span>
                    <span v-else>{{ adultAmount }} guests</span>
                </button>
                <reservation-dropdown>
                    <div v-if="active" id="myDropdown" class="dropdown-content flex">
                        <span>adults</span>
                        <div>
                            <button v-if="adultAmount > 1" @click="inc(-1)">-</button>
                            <span class="adult-ctr">{{ adultAmount }}</span>
                            <button v-if="adultAmount < 9" @click="inc(1)">+</button>
                        </div>
                    </div>
                </reservation-dropdown>
            </div>
        </div>
        <button class="reserve-btn" @click="reserveOrder"> Reserve </button>
        <div class="reservation-total-price" v-if="nights">
            <span>you will not be charged yet</span>
            <div class="total">
                <span>${{ stay.price }} x {{ nights }} nights</span><span>${{ totalPrice }}</span>
            </div>
            <div class="total-last">
                <span>service fee</span><span>$300</span>
            </div>
            <p>
                <span>Total</span><span>${{ totalPrice + 300 }}</span>
            </p>
        </div>
        <!-- <airbnb-btn /> -->

    </div>
</template>

<script>
import reservationDropdown from '../details/reservation-dropdown.cmp.vue'
import datePicker from '../home/date-picker.cmp.vue'
import { showSuccessMsg } from '../../services/event-bus.service.js'
import { socketService } from '../../services/socket.service'
// import airbnbBtn from '../airbnb-btn.cmp.vue';

export default {
    props: {
        stay: {
            type: Object,
            required: true,
        },
    },
    data() {
        return {
            active: false,
            adultAmount: 1,
            date: [],
            totalPrice: 0,
            nights: null,
            loggedinUser:{},
            
        };
    },
    created() {
        this.loggedinUser = this.$store.getters.loggedinUser
        console.log(this.loggedinUser)
        this.date = this.$store.getters.getCurrDate
        console.log(this.date)
        if (this.date.length) this.changeDate()
        this.hover

    },
    methods: {
        getCreateAt() {
            let hours = new Date().getHours()
            let min = new Date().getMinutes()
            let day = new Date().getDate();
            let munth = new Date().getMonth()
            let createAt = `${day}/${munth}  ${hours}:${min}`
            return createAt



        },
        toggle() {
            this.active = !this.active
        },
        inc(num) {
            this.adultAmount += num
        },
        reservationValue() {
            const _MS_PER_DAY = 1000 * 60 * 60 * 24;
            // Discard the time and time-zone information.
            const utc1 = Date.UTC(this.date[0].getFullYear(), this.date[0].getMonth(), this.date[0].getDate());
            const utc2 = Date.UTC(this.date[1].getFullYear(), this.date[1].getMonth(), this.date[1].getDate());
            console.log(Math.floor((utc2 - utc1) / _MS_PER_DAY))
            this.nights = (Math.floor((utc2 - utc1) / _MS_PER_DAY))
            console.log('here before');
            console.log(this.date)
        },
        async reserveOrder() {
            if (!this.date.length) {
                showSuccessMsg(`please choose date!`)
                return
            }
            const orderDet = {

                checkin: this.date[0],
                checkout: this.date[1],
                guests: this.adultAmount,
                guestsDetails: {
                    guestId: this.loggedinUser._id,
                    guestName: this.loggedinUser.fullname
                },
                price: this.stay.price,
                totalPrice: this.totalPrice,
                stayId: this.stay._id,
                totalNight: this.nights,
                host: {
                    hostId: this.stay.host.id,
                    hostName: this.stay.host.fullname,
                },
                stay: {
                    name: this.stay.name
                },
                status:'pending',
                createAt:this.getCreateAt(),



                
            }
            showSuccessMsg(`reserved ${this.nights} nights succsesfully `)
            await this.$store.dispatch({
                type: 'saveOrder',
                order: orderDet
            })

        },
        getTotalPrice() {
            this.totalPrice = +this.stay.price * this.nights
        },
        changeDate() {
            this.reservationValue()
            this.getTotalPrice()
        },
    },
    computed: {

    },
    unmounted() { },
    components: {
        reservationDropdown,
        datePicker,
        // airbnbBtn
    }
};
</script>
<style >
</style>