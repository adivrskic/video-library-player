<template>
  <div class="container">
    <Logo />
    <div @click="openMenu" class="menu-container">
        <Hamburger />
    </div>

    <Menu v-if="showMenuModal" />
  </div>
</template>

<script>
import Logo from '../assets/Logo'
import Hamburger from '../assets/Hamburger'
import Menu from './Menu'

import { eventBus } from '../main.js'

export default {
    data () {
        return {
            showMenuModal: false
        }
    },
    components: {
        Hamburger,
        Logo,
        Menu
    },
    methods : {
        openMenu() {
            this.showMenuModal = !this.showMenuModal;
            if(document.body.className.indexOf('no-overflow') > -1) {
                document.body.className = document.body.className.replace("no-overflow","");
            } else {
                document.body.className += ' ' + 'no-overflow';
            }
        }
    },
    created() {
        eventBus.$on('closeModal', val => {
            this.showMenuModal = val;
        })
    }
}
</script>

<style scoped>
    .container {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background: #456aaf;
        height: 2.5rem;
        padding: 1rem;
    }

    .menu-container {
        cursor: pointer;
    }

    svg {
        position: relative;
        height: 100%;
        width: 2.5rem;
    }
</style>
