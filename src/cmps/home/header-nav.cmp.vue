<template  v-if="isShowDropdownMenu" >
    <div class="main-nav" v-click-outside="closeDropdownMenu">
        <nav>
            <router-link to="/explore">Explore</router-link>
            <router-link to="/host" :user="user">Become a Host</router-link>
            <button class="user-menu-btns clickable" @click="showMenu = !showMenu">
                <!-- <i class="fa-solid fa-bars"></i> -->
                <svg viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" role="presentation"
                    focusable="false"
                    style="display: block;fill: none;height: 16px;width: 16px;stroke: currentcolor;stroke-width: 3;overflow: visible;">
                    <g fill="none" fill-rule="nonzero">
                        <path d="m2 16h28"></path>
                        <path d="m2 24h28"></path>
                        <path d="m2 8h28"></path>
                    </g>
                </svg>
                <img src="/img/peopleImages/1003.jpg" class="avatar-header " />
            </button>
        </nav>

        <div class="user-nav-container" v-if="showMenu">
            <div class="user-nav flex column">
                <!-- <button @click="login">login</button> -->
                <!-- <router-link to="#">Log in</router-link> -->
                <router-link to="#">Notifications</router-link>
                <!-- <router-link v-if="user" @click="goToBackOffice">Orders</router-link> -->
                <router-link v-if="user" to="/orders">Orders</router-link>
                <router-link :stays="stays" to="/wishlist">Wishlist</router-link>
                {{ loginSignup }}
            </div>
        </div>

    </div>
</template>
<script>


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
            adults: 0,
            children: 0,
            shouldShow: false,
            isShowDropdownMenu: false,
            dates: [],
            user: null,
            stays:null,
        };
    },
    // async created() {
    //     this.stays = this.$store.getters.stays
    //     this.$store.dispatch({ type: "setUser" });
    //     try {
    //         const loggedinUser = await this.$store.getters.loggedinUser
    //         this.user = loggedinUser
    //         console.log(this.user)
    //     } catch (err) {
    //         console.log("userStore: Error in login", err)
    //         throw err
    //     }
    created() {
    },

    methods: {
        closeDropdownMenu() {
            console.log(this.isShowDropdownMenu);
            this.isShowDropdownMenu = false
        },
        // async login() {
        //     var number = +prompt("login")
        //     try {
        //         this.$store.dispatch({ type: 'loadUsers', num: number })
        //         const loggedinUser = await this.$store.getters.loggedinUser
        //         this.user = loggedinUser
        //     } catch (err) {
        //         console.log("userStore: Error in login", err)
        //         throw err
        //     }
        // }
    },

    computed: {},
    unmounted() { },
    components: {

    },
};
</script>
<style lang="">
    
</style>