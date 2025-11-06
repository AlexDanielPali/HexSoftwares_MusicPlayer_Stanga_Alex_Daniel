<template>
    <div class="player-app">
        <div class="playlist-panel">
            <h2 class="app-title text-yellow-300 font-bold text-2xl">Music Application</h2>

            <ul class="playlist">
                <li
                    v-for="(song, songIndex) in list"
                    :key="song.id"
                    :class="{ active: songIndex === currentSongIndex }"
                    @click="playSong(songIndex)"
                >
                    <div class="meta">
                        <div class="title">{{ song.name }}</div>
                        <div class="artist">{{ song.artistName }} · {{ song.albumName }} ({{ song.year }})</div>
                    </div>
                    <div class="duration">
                        <img class="rounded" :src="song.src" style="height:48px;width:48px;object-fit:cover" />
                    </div>
                </li>
            </ul>
        </div>

        <div class="control-panel">
            <SongPlayer
                v-bind:song="list[currentSongIndex]"
                @goback="isPlayerVisible = !isPlayerVisible"
                @next="playNext"
                @previous="playPrevious"
            />
        </div>
    </div>
</template>

<script>
import SongPlayer from './SongPlayer.vue';

export default {
    data () {
        return {
            isPlayerVisible: false,
            currentSongIndex: 0,
            list: [
                {
                    id: 1,
                    name: 'SoundHelix Song 1',
                    artistName: 'SoundHelix',
                    albumName: 'SoundHelix',
                    year: 2025,
                    src: `https://images.pexels.com/photos/1389429/pexels-photo-1389429.jpeg`,
                    songSrc: `https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3`
                },
                {
                    id: 2,
                    name: 'SoundHelix Song 2',
                    artistName: 'SoundHelix',
                    albumName: 'SoundHelix',
                    year: 2025,
                    src: `https://images.pexels.com/photos/1389429/pexels-photo-1389429.jpeg`,
                    songSrc: 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3'
                },
                {
                    id: 3,
                    name: 'SoundHelix Song 3',
                    artistName: 'SoundHelix',
                    albumName: 'SoundHelix',
                    year: 2025,
                    src: `https://images.pexels.com/photos/1389429/pexels-photo-1389429.jpeg`,
                    songSrc: `https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3`
                }
            ]
        }
    },
    methods: {
        playSong (index) {
            this.currentSongIndex = index;
            this.isPlayerVisible = true;
        },
        playNext () {
            if (this.currentSongIndex < this.list.length - 1) {
                this.currentSongIndex += 1;
            } else {
                this.currentSongIndex = 0;
            }
        },
        playPrevious () {
            if (this.currentSongIndex !== 0) {
                this.currentSongIndex -= 1;
            } else {
                this.currentSongIndex = this.list.length - 1;
            }
        }
    },
    components: { SongPlayer },
    name: 'SongList'
}
</script>