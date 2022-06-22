<template>
    <section class="page_main">
        <header-component 
            :toggleSideBar="toggleSideBar"
        />

        <!-- Side Bar Section -->
        <side-bar-component :showSideBar="showSideBar" :menus="menus"/>

        <!-- Main page Section -->
        <section class="page_list page_anim" @scroll="(ev) => handlePageScroll(ev)">

            <h1 v-if="isLoading">Loading...</h1>

            <card-component v-else
                v-for="carddata in cardData" 
                :key="carddata.id"
                :cardData="carddata"
            />

        </section>
    </section>
</template>

<script>
import CardComponent from '@/components/Card/CardComponent.vue';
import HeaderComponent from '../components/Header/HeaderComponent.vue';
import SideBarComponent from '../components/SideBar/SideBarComponent.vue';

export default {
    name: 'HomePage',
    
    data() {
        return {
            showSideBar: false,

            isLoading: true,

            messageCallStatus: false,

            menus: [
                {
                    key: 'home',
                    title: 'Home'
                },
                {
                    key: 'msg',
                    title: 'Message'
                },
                {
                    key: 'report',
                    title: 'Reports'
                },
                {
                    key: 'settings',
                    title: 'Settings'
                },
                {
                    key: 'profile',
                    title: 'Profile'
                }
            ],

            cardData: []

        }
    },

    created() {
        this.getMessages();
    },

    methods: {
        getMessages()
        {
            this.messageCallStatus = true;

            fetch('https://message-list.appspot.com/messages')
                .then( (response) => response.json() )
                .then( (response) => {
                    this.cardData.push( ...response.messages );
                    this.isLoading = false;
                    this.messageCallStatus = false;
                });
        },

        toggleSideBar() 
        {
            this.showSideBar = !this.showSideBar;
        },

        handlePageScroll( { target } ) 
        {
            if( this.messageCallStatus ) { retrun; }

            if ((target.clientHeight + target.scrollTop) > target.scrollHeight - 50 ) 
            {
                this.getMessages();
            }
        }
    },

    components: {
        HeaderComponent,
        SideBarComponent,
        CardComponent
    }
}
</script>