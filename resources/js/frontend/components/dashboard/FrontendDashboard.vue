<template>
    <div>
        <layout-master :disable-sidebar="showSidebar" v-if="messageBus.isReady()">
            <v-breadcrumbs :items="breadcrumbItems" divider=">"></v-breadcrumbs>
            <router-view></router-view>
        </layout-master>
        <loading-eclipse v-if="!messageBus.isReady()"></loading-eclipse>
    </div>

</template>

<script>
    import LayoutMaster from "./layout/Master";
    import LoadingEclipse from "../../../vue-tools/LoadingEclipse";
    import {MessageBus} from "../../../vue-tools/message-bus";

    export default {
        name: 'frontend-dashboard',
        components: {
            LoadingEclipse,
            LayoutMaster,
        },
        data(){
            return{
                breadcrumbItems : this.$route.meta.breadcrumb,
                messageBus: MessageBus,
            }
        },
        props: {
            session: { // json string contains the current session info
                type: String,
                required : true,
            }
        },
        computed: {
            showSidebar(){
                // if is in control then show
                if(this.$router.full)
                    return true;
                else
                    return false;
            }
        },
        watch: {
            '$route'(){
                _.forEach(this.$route.meta.breadcrumb, function (value, index) {
                    let length = this.$route.meta.breadcrumb.length;
                    if(length -1 !== index) // if not the last item
                        this.$route.meta.breadcrumb[index].disabled = false;
                    else
                        this.$route.meta.breadcrumb[index].disabled = true;
                }.bind(this));
                this.$route.meta.breadcrumb[this.$route.meta.breadcrumb.length-1].disabled = true;
                this.breadcrumbItems = this.$route.meta.breadcrumb;
            }
        },
        methods: {


        },
        mounted(){

            MessageBus.setSession(this.session);

            // to route to the correct page if required
            let url = new URL(window.location.href);
            let routeName = url.searchParams.get("to");
            if(routeName){
                this.$router.push({
                    name: routeName
                })
            }

        }
    }
</script>

<style>
</style>
