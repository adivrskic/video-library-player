<template>
    <div id="modal" class="menu-modal-container" @click="closeModal">
        <div id="menu-modal" class="menu-modal-content">
            <button @click="resetApplication" type="button">Reset</button>
        </div>
  </div>
</template>

<script>
    import { eventBus } from '../main.js'

    import userInfoJson from '../../userinfo.json'

    const fs = require('fs');

    export default {
        data() {
            return {
              userInfoJson: userInfoJson
            }
        },
        methods: {
            closeModal(e) {
                let modal = document.getElementById('modal');
                if(e.target === modal) {
                    eventBus.$emit('closeModal', false)
                }  
            },
            resetApplication() {
                fs.writeFileSync('./userinfo.json', JSON.stringify({}));
            }
        }
    }
</script>

<style scoped>
    .menu-modal-container {
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
        background: rgba(0,0,0,.5);
        pointer-events: all;
        z-index: 10;
    }

    .menu-modal-content {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 80%;
        max-width: 800px;
        height: 80%;
        background: #fff;
        z-index: 10;
        pointer-events: all;
        overflow-y: scroll;
        scrollbar-width: none;
    }

    .menu-modal-content::-webkit-scrollbar {
      display: none;
    }

    button {
      padding: 1rem 2rem;
      background: #456aaf;
      opacity: .7;
      color: #fff;
      border: none;
      cursor: pointer;
      text-transform: uppercase;
      font-size: 1.5rem;
      transition: all .2s ease-in-out
    }

    button:hover,
    button:focus {
      opacity: 1;
    }
</style>