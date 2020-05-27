<template>
  <div>  
    <video width="100%" height="100%" controls autoplay>
      <source :src="currentEpisode" type="video/mp4">
      Video tag not supported
    </video>
    <button class="close" @click="closePlayer">X</button>
  </div>

</template>

<script>
import { eventBus } from '../main';

export default {
    props: ['path', 'currentEpisode'],
    methods: {
      closePlayer() {
        console.log('hello')
        eventBus.$emit('closeVideoPlayer', false);
      }
    }
}
</script>

<style scoped>
video {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 99;
    background: #000;
}

.close {
    background: none;
    border: none;
    color: #fff;
    z-index: 100;
    position: absolute;
    top: .25rem;
    right: .25rem;
    padding: .25rem .45rem;
    opacity: 0;
    pointer-events: all;
    border-radius: 50%;
    cursor: pointer;
    transition: all .2s ease-in-out;
}

video:hover + .close,
.close:hover {
    opacity: 1;
    background: rgba(255,255,255, .2);
}
</style>
