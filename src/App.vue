<template>
    <div class="bg-[#F8FAFC] dark:bg-[#0A1122] w-screen h-screen flex flex-col items-center justify-center">
        <div
            class="relative w-player flex flex-col rounded-xl shadow-player-light bg-player-light-background border border-player-light-border dark:shadow-player-dark dark:bg-player-dark-background dark:border-player-dark-border dark:backdrop-blur-xl">
            <div class="px-10 pt-10 pb-4 flex items-center z-50">
                <div class="w-full">
                    <label for="video_id" class="text-gray-400 text-sm">
                        Video Url
                    </label>
                    <div class="flex">
                        <div class="relative w-full">
                            <input v-model="video_id" id="video_id" placeholder="Video Url" type="text"
                                class="h-10 rounded dark:bg-gray-800 focus:ring-1 pl-4 border-gray-300 focus:border-gray-300 dark:text-gray-400 focus:border-none placeholder:pl-4 border dark:border-gray-600 w-full mb-4">
                            <button @click="playCurrentVideo()" type="submit"
                                class="absolute top-0 h-10 right-0 p-2.5 text-sm font-medium text-white bg-blue-700 rounded-r border border-blue-700 hover:bg-blue-800 focus:ring-1 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                                Play
                            </button>
                        </div>
                    </div>
                    <YoutubeVue3 class="w-full h-48 object-cover rounded" ref="youtube" :videoid="video_id" :loop="loop"
                        :width="480" :height="320" @ended="onEnded" @paused="onPaused" :controls="1" @played="onPlayed" />
                </div>
            </div>
            <div
                class="h-control-panel px-10 rounded-b-xl bg-control-panel-light-background border-t border-gray-200 flex items-center justify-between z-50 dark:bg-control-panel-dark-background dark:border-gray-900">
                <div @click="loopSong()" class="dark:text-gray-400 cursor-pointer amplitude-shuffle"
                    :class="loop ? 'text-blue-500 dark:text-blue-600' : ''">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="w-8 h-8">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0l3.181 3.183a8.25 8.25 0 0013.803-3.7M4.031 9.865a8.25 8.25 0 0113.803-3.7l3.181 3.182m0-4.991v4.99" />
                    </svg>
                </div>
                <template v-if="play_status">
                    <div @click="pauseCurrentVideo()"
                        class="cursor-pointer dark:text-gray-400 amplitude-play-pause w-24 h-24 rounded-full bg-white border border-play-pause-light-border shadow-xl flex items-center justify-center dark:bg-play-pause-dark-background dark:border-play-pause-dark-border">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-12 h-12">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M14.25 9v6m-4.5 0V9M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                </template>
                <template v-else>
                    <div @click="playCurrentVideo()"
                        class="cursor-pointer dark:text-gray-400 amplitude-play-pause w-24 h-24 rounded-full bg-white border border-play-pause-light-border shadow-xl flex items-center justify-center dark:bg-play-pause-dark-background dark:border-play-pause-dark-border">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-12 h-12">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M15.91 11.672a.375.375 0 010 .656l-5.603 3.113a.375.375 0 01-.557-.328V8.887c0-.286.307-.466.557-.327l5.603 3.112z" />
                        </svg>
                    </div>
                </template>
                <div @click="dark_mode()" class="dark:text-gray-400 cursor-pointer amplitude-shuffle">
                    <template v-if="dark">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-8 h-8">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z" />
                        </svg>
                    </template>
                    <template v-else>
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-8 h-8">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M21.752 15.002A9.718 9.718 0 0118 15.75c-5.385 0-9.75-4.365-9.75-9.75 0-1.33.266-2.597.748-3.752A9.753 9.753 0 003 11.25C3 16.635 7.365 21 12.75 21a9.753 9.753 0 009.002-5.998z" />
                        </svg>
                    </template>

                </div>
            </div>
            <div
                class="hidden top-14 w-full absolute ml-auto mr-auto left-0 right-0 text-center max-w-lg h-72 rounded-full bg-highlight blur-2xl dark:block">
            </div>
        </div>
    </div>
</template>

<script>
import { YoutubeVue3 } from 'youtube-vue3'

export default {
    name: 'App',
    components: {
        YoutubeVue3,
    },
    data() {
        return {
            dark: false,
            loop: 0,
            video_id: 'Xz52VqNrA2o',
            play_status: false,
            pause_status: false,
        }
    },
    mounted() {
        var theme = localStorage.getItem('my-yt-player-theme')
        if (theme == 'true') {
            document.body.classList.add("dark");
            this.dark = true;
        }
    },
    methods: {
        applyConfig() {
            // this.play = Object.assign(this.play)
        },
        playCurrentVideo() {
            this.$refs.youtube.player.playVideo();
            this.play_status = true;
            this.pause_status = false;
        },
        stopCurrentVideo() {
            this.$refs.youtube.player.stopVideo();
        },
        pauseCurrentVideo() {
            this.$refs.youtube.player.pauseVideo();
            this.play_status = false;
            this.pause_status = true;
        },
        onEnded() {
            console.log("## OnEnded")
        },
        onPaused() {

        },
        onPlayed() {

        },
        dark_mode() {
            if (document.body.classList.contains("dark")) {
                document.body.classList.remove("dark");
                localStorage.setItem('my-yt-player-theme', false)
                this.dark = false;
            } else {
                document.body.classList.add("dark");
                localStorage.setItem('my-yt-player-theme', true)
                this.dark = true;
            }
        },
        loopSong() {
            if (this.loop == true) {
                this.loop = false;
            } else {
                this.loop = true;
            }
        }
    }
}
</script>
