<template>
    <div id="app">
        <Header />
        <component :is="currentComponent"></component>
    </div>
</template>

<script>
    import { eventBus } from './main.js'
    import userInfoJson from '../userinfo.json'

    import Header from './components/Header'
    import Home from './components/Home'
    import Initialize from './components/Initialize'

    export default {
        name: 'App',
        data () {
            return {
                currentComponent: '',
                userInfoJson: userInfoJson
            }
        },
        components: {
            Header,
            Home,
            Initialize
        },
        methods: {
            checkForUserInfoFile: function() {
                if(Object.keys(this.userInfoJson).length === 0 && this.userInfoJson.constructor === Object) {
                    this.currentComponent = 'Initialize'
                } else {
                    this.currentComponent = 'Home'
                }
            }
        },
        beforeMount() {
            this.checkForUserInfoFile();
        },
        created() {
            eventBus.$on('switchComponent', component => {
                this.currentComponent = component;
            })
        },
        mounted() {
            eventBus.$on('switchComponent', component => {
                this.currentComponent = component;
            })
        }
    }
</script>

<style>
    body, #app {
        width: 100%;
        height: 100%;
        min-height: 100vh;
        margin: 0;
        padding: 0;
        font-size: 16px;
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        color: #444;
        overflow: hidden;
    }

    #app {
        background-color: #fff;
    }

    .no-overflow {
        overflow: hidden;
    }
</style>
