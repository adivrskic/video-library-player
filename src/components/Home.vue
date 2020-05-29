<template>
    <div>
        <div class="grid">
            <div class="grid-item" 
                 v-for="item in this.tree.children" 
                 v-bind:key="Math.random() + item"
                 @click="showEpisodes(item.path, item.children[0])"
            >
                <img v-if="item.children[1]" 
                     class="grid-item-image" 
                     :src="item.path + '/' + item.children[1].name" 
                >
                <h2 v-else 
                    class="grid-no-image">
                    {{ noImageText }}
                </h2>
                
                <p class="grid-item-title">{{ item.name }}</p>
                <div class="grid-item-overlay"></div>
            </div>
        </div>

        <EpisodeList 
            v-if="showEpisodesModal" 
            :path="dir" 
            :episodes="episodes" 
        />

        <VideoPlayer
            v-if="showVideoPlayer" 
            :path="dir" 
            :currentEpisode="currentEpisode" 
        />
    </div>
</template>

<script>
    import { eventBus } from '../main.js'

    import EpisodeList from './EpisodeList';
    import VideoPlayer from './VideoPlayer';

    const dirTree = require('directory-tree');

    export default {
        data () {
            return {
                dir:'C:/Users/Adi/Videos/anime',
                tree: '',
                episodes: '',
                currentEpisode: '',
                showEpisodesModal: false,
                showVideoPlayer: false,
                noImageText: 'No Image Available'
            }
        },
        components: {
            EpisodeList,
            VideoPlayer
        },
        methods: {
            readDirectory() {
                this.tree = dirTree(this.dir);
            },
            showEpisodes(path, episodes) {
                this.episodes = episodes;
                this.showEpisodesModal = !this.showEpisodesModal;
                if(document.body.className.indexOf('no-overflow') > -1) {
                    document.body.className = document.body.className.replace("no-overflow","");
                } else {
                    document.body.className += ' ' + 'no-overflow';
                }
            }
        },
        created() {
            this.readDirectory(),
            eventBus.$on('closeModal', val => {
                this.showEpisodesModal = val;
            }),

            eventBus.$on('showVideoPlayer', val => {
                this.currentEpisode = val.episode;
                this.showVideoPlayer = val.showVideoPlayer;
            }),

            eventBus.$on('closeVideoPlayer', val => {
                this.showVideoPlayer = val
            })
        }
    }
</script>

<style scoped>
    body.no-overflow {
        overflow: hidden !important;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(20.250rem, 1fr)); 
        grid-auto-rows: 17.25rem;
        grid-gap: 1em;
        color: #fff;
        padding: 1rem;
        height: calc(100vh - 7.25rem);
        overflow-y: scroll;
    }

    .grid-item {
        cursor: pointer;
        position: relative;
    }

    .grid-item-overlay {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        transition: all .2s ease-in-out
    }

    .grid-item:hover .grid-item-overlay {
        background: rgba(0,0,0,.5);
    }

    .grid-item:hover .grid-item-title {
        opacity: 1;
    }

    .grid-item-image {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .grid-item-title {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100%;
        margin: 0;
        position: absolute;
        top: 0;
        left: 0;
        opacity: 0;
        text-transform: uppercase;
        font-size: 1.25rem;
        font-weight: 500;
        text-align: center;
        transition: all .2s ease-in-out;
        z-index: 3;
    }

    .grid-no-image {
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        height: 100%;
        margin: 0;
        font-size: 2.5rem;
        color: #ddd;
    }

    /* width */
    ::-webkit-scrollbar {
        width: .33rem;
    }

    /* Track */
    ::-webkit-scrollbar-track {
        background: #fff;
    }

    /* Handle */
    ::-webkit-scrollbar-thumb {
        background: #ddd;
    }
</style>
