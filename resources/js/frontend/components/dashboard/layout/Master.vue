<template>
    <v-app :dark="dark">
        <logged-in-as-alert/>

        <sidebar v-if="!disableSidebar" v-model="showSidebar"></sidebar>

        <navbar v-if="!disableSidebar" @toggled-sidebar="showSidebar=!showSidebar"></navbar>

        <v-content>
            <slot></slot>
        </v-content>
    </v-app>

</template>

<script>
    import Sidebar from "./sidebar/BaseSidebar";
    import Navbar from "./NavBar";
    import {EventBus} from "../../../../vue-tools/event-bus";
    import LoggedInAsAlert from "../../../../vue-tools/LoggedInAsAlert";


    export default {
        name: "layout-master",
        components: {LoggedInAsAlert, Navbar, Sidebar},
        data() {
            return {
                dark: false,
                showSidebar: null,
            }
        },
        props: {
            disableSidebar: {
                type: Boolean,
                default: false
            }
        },
        methods: {

        },
        mounted() {
            EventBus.$on('toggled-dark', function () {
                this.dark = !this.dark;
            }.bind(this));

            EventBus.$on('toggled-sidebar', function () {
                this.showSidebar = !this.showSidebar;
            }.bind(this))
        },
    }

</script>

<style scoped>

</style>
