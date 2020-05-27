<template>
  <div id="modal" class="container" @click="closeModal">
      <div class="content">
          <ul>
                <li v-for="episode in episodes.children" 
                    :key="episode.name"
                    @click="openPlayer(episode.path)">
                    <input type="checkbox" /> 
                    {{ episode.name }}
                </li>
          </ul>
          <h2 v-if="Object.keys(episodes.children).length == 0">{{ noVideosMessage }}</h2>
      </div>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
    data() {
        return {
            noVideosMessage: 'Sorry, there were no videos to load'
        }
    },
    props: ['path', 'episodes'],
    methods: {
        closeModal: function(e) {
            let modal = document.getElementById('modal');
            if(e.target === modal) {
                eventBus.$emit('closeModal', false)
            }
            
        },
        openPlayer(path) {
            console.log(path, name)
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
    background: rgba(85, 122, 149, .2)
}

h2 {
    padding: 1rem;
    margin: 0;
}
</style>
