<template>
    <div>
        <!-- hidden audio element -->
        <audio ref="audioPlayer" :src="song.songSrc" preload="metadata"></audio>

        <div class="now-playing">
            <div class="track-info">
                <div class="title">{{ song.name }}</div>
                <div class="artist">{{ song.artistName }} — {{ song.albumName }}</div>
            </div>
            <div class="track-art">
                <img :src="song.src" alt="cover" style="height:64px;width:64px;border-radius:8px;object-fit:cover" />
            </div>
        </div>

        <div class="visualizer playing mt-4" aria-hidden="true">
            <span class="wave"></span>
            <span class="wave"></span>
            <span class="wave"></span>
            <span class="wave"></span>
            <span class="wave"></span>
        </div>

        <div class="progress-wrap mt-4">
            <div style="display:flex;justify-content:space-between;font-size:0.9rem;color:var(--muted);margin-bottom:6px">
                <div>{{ formattedCurrentTime }}</div>
                <div>{{ formattedDuration }}</div>
            </div>
            <div class="progress-bar" @click="seek($event)">
                <div class="progress" :style="{ width: progressPercent + '%' }"></div>
            </div>
        </div>

        <div class="controls mt-6">
            <button class="btn" @click="toggleRepeat" :title="repeat ? 'Repeat: on' : 'Repeat: off'">
                <span :style="{ color: repeat ? 'var(--accent)' : 'inherit' }">⤾</span>
            </button>

            <button class="btn" @click="previous">⟸</button>

            <button class="btn primary" @click="togglePlay">
                {{ isPlaying ? 'Pause' : 'Play' }}
            </button>

            <button class="btn" @click="next">⟹</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SongPlayer',
    props: {
        song: {
            type: Object,
            required: true
        }
    },
    emits: ['goback', 'next', 'previous'],
    data () {
        return {
            isPlaying: false,
            duration: 0,
            currentTime: 0,
            repeat: false
        }
    },
    computed: {
        progressPercent () {
            if (!this.duration) return 0;
            return (this.currentTime / this.duration) * 100;
        },
        formattedCurrentTime () {
            return this.formatTime(this.currentTime);
        },
        formattedDuration () {
            return this.formatTime(this.duration);
        }
    },
    watch: {
        song: {
            immediate: true,
            handler () {
                // when the song prop changes, load it and play
                const audio = this.$refs.audioPlayer;
                if (!audio) return;
                audio.pause();
                audio.currentTime = 0;
                this.currentTime = 0;
                this.duration = 0;
                // load new src
                // small timeout to ensure src prop applied
                this.$nextTick(() => {
                    try {
                        audio.load();
                        // attempt to play only if songSrc present
                        if (this.song.songSrc) {
                            audio.play().then(() => { this.isPlaying = true }).catch(() => { this.isPlaying = false });
                        }
                    } catch (e) {
                        // ignore
                    }
                });
            }
        }
    },
    mounted () {
        const audio = this.$refs.audioPlayer;
        if (!audio) return;

        audio.addEventListener('loadedmetadata', () => {
            this.duration = audio.duration || 0;
        });

        audio.addEventListener('timeupdate', () => {
            this.currentTime = audio.currentTime;
        });

        audio.addEventListener('ended', () => {
            if (this.repeat) {
                audio.currentTime = 0;
                audio.play();
            } else {
                this.$emit('next');
            }
        });
    },
    methods: {
        goback () { this.$emit('goback'); },
        togglePlay () {
            const audio = this.$refs.audioPlayer;
            if (!audio) return;
            if (this.isPlaying) {
                audio.pause();
            } else {
                audio.play();
            }
            this.isPlaying = !this.isPlaying;
        },
        next () { this.$emit('next'); },
        previous () { this.$emit('previous'); },
        toggleRepeat () { this.repeat = !this.repeat; },
        formatTime (seconds = 0) {
            if (!seconds || isNaN(seconds)) return '0:00';
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60).toString().padStart(2, '0');
            return `${mins}:${secs}`;
        },
        seek (event) {
            const rect = event.currentTarget.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const pct = x / rect.width;
            const audio = this.$refs.audioPlayer;
            if (!audio || !this.duration) return;
            audio.currentTime = pct * this.duration;
        }
    }
}
</script>