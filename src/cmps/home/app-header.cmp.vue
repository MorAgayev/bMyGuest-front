<template>
    <div class="app-header" :class="{
        'header-container': !miniSearch,
        'close-header': miniSearch,
    }">
        <section :class="{ 'stay-details': stayDetails, 'explore-page': explorePage, 'home-page': homePage, }">
            <section class="top-header main-layout flex">
                <div class="logo">
                    <h1 @click.stop="goHome">
                        <span>
                            <i class="fa-brands fa-airbnb logo-icon"></i>
                        </span>
                        <span class="logo-txt">
                            BmyGuest
                        </span>
                    </h1>
                </div>
                <div v-if="miniSearch" class="mini-search clickable" @click="openSearchBar">
                    <label>Anywhere</label>
                    <label>Any week</label>
                    <label>Add guests</label>
                    <span></span>
                </div>
                <header-nav />
            </section>
            <open-header v-if="!miniSearch" />
        </section>
    </div>
</template>

<script>
import datePicker from "./date-picker.cmp.vue"
// import LoginSignupCmp from "../login-signup.cmp.vue"
import headerNav from "./header-nav.cmp.vue"
import openHeader from "./open-header.cmp.vue"
// import { eventBus } from "../services/eventBus.js"
export default {
    data() {
        return {
            showMenu: false,
            scrolldown: 0,
            openSearch: false,
            miniSearch: true,
            hideSearch: false,
            pageTop: true,
            currPage: null,
            loginSignup: false,
            adults: 0,
            children: 0,
            shouldShow: false,
            dates: [],
        }
    },
    created() {
        window.addEventListener("scroll", this.handleScroll)
        this.miniSearch = true
        this.$emit("minisearch",true)
        // if(this.$store)
    },
    mounted() {
        this.miniSearch = true
    },
    methods: {
        goHome() {
            this.$router.push('/')
        },
        handleScroll() {
            if (window.scrollY > this.pageTop + 120 || window.scrollY < this.pageTop) {
                this.miniSearch = true
                this.$emit('openHeader', false)
            }
        },
        openSearchBar() {
            this.miniSearch = false
            this.pageTop = window.top.scrollY
            this.$emit('openHeader', true)
        },
        openLoginModal() {
            // this.loginSignup = true
        },

    },
    computed: {
        stayDetails() {
            const res = this.currPage === 'stayDetails' ? true : false
            console.log(this.currPage)
            return res
        },
        explorePage() {
            const res = this.currPage === 'explorePage' ? true : false
            console.log(this.currPage)
            return res
        },
        homePage() {
            const res = this.currPage === 'homePage' ? true : false
            console.log(this.currPage)
            return res
        }
    },
    components: {
        datePicker,
        // LoginSignupCmp,
        headerNav,
        openHeader,
    },
    unmounted() {
        window.removeEventListener("scroll", this.handleScroll)
    },
    watch: {
        "$store.state.currPage": {
            handler() {
                this.currPage = this.$store.getters.currPage
                this.miniSearch = this.currPage !== "homePage" && this.currPage !== "explore" 
                if(this.currPage === "homePage") this.miniSearch = true
                // this.hideFilter = this.currPage !== "homePage" && this.currPage !== "explore" && this.currPage !== "stayDetails"
            },
            immediate: true,
            deep: true,
        },
    },
}
</script>
    