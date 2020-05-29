<template>
  <div id="modal" class="container" @click="closeModal">
      <div id="episode-modal" class="content">
            <ul>
                    <li v-for="episode in episodes.children" 
                        :key="Math.random() + episode.name"
                        @click="openPlayer(episode.path)">
                        <input type="checkbox" /> 
                        {{ episode.name }}
                    </li>
            </ul>
            <div v-if="Object.keys(episodes.children).length == 0"
                class="error"
            >
                <Alert />
                <h2 >{{ noVideosMessage }}</h2>
            </div>
      </div>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

import Alert from '../assets/Alert'

export default {
    data() {
        return {
            noVideosMessage: 'Sorry, couldn\'t find any .mp4 videos'
        }
    },
    components: {
        Alert
    },
    props: ['path', 'episodes'],
    methods: {
        closeModal(e) {
            let modal = document.getElementById('modal');
            if(e.target === modal) {
                eventBus.$emit('closeModal', false)
            }
            
        },
        openPlayer(path) {
            eventBus.$emit('showVideoPlayer', {
                'showVideoPlayer': true,
                'episode': path
            })
        }
    }
}
</script>

<style scoped>
.container {
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

.content {
    width: 80%;
    max-width: 800px;
    height: 80%;
    background: #fff;
    z-index: 10;
    pointer-events: all;
    overflow-y: scroll;
    scrollbar-width: none;
}

.content::-webkit-scrollbar {
  display: none;
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

li {
    font-size: 1.25rem;
    padding: 2rem;
    cursor: pointer;
    transition: all .2s ease-in-out;
}

li:hover {
    background: rgba(204,204,204,.2)
}

.error {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 1rem;
    height: 100%;
}

h2 {
    padding: 1rem;
    margin: 0;
    color: #ccc;
}

svg {
    width: 75%;
    height: 13.75rem;
    margin-bottom: 2.5rem;
}
</style>
