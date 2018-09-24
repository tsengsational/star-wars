<template>
    <div class="film-wrapper" >
        <div class="film" @click="toggleScrawl" >
            {{film.title}}
            <div class="info" >
                <div class="release">
                    {{releaseDate}}
                </div>
            </div>
        </div>
        <div class="scrawl-wrapper" @click="toggleScrawl" :class="{fade: showScrawl}" >
            <scrawl :film="film" v-if="showScrawl"></scrawl>
        </div>
    </div>
</template>

<script>
import moment from "moment";
import Scrawl from "./Scrawl"

export default {
    props: ["url"],
    components: {
        Scrawl
    },
    data() {
        return {
            film: null,
            showScrawl: false,
        }
    },
    created() {
        console.log(this.url)
        const url = this.url
        this.getFilm(url);
    },
    computed: {
        releaseDate() {
            let date = new Date(this.film.release_date)
            const day = date.getDate();
            const month = date.getMonth();
            const year = date.getYear();
            return moment(this.film.release_date).format("dddd, MMMM Do YYYY")
        }
    },
    watch: {
        url: function(val) {
            console.log("fetching again:", val),
            this.getFilm(val)
        }
    },
    methods: {
        getFilm(url) {
            fetch(url)
            .then(response => {
                return response.json()
            })
            .then(json => {
                this.film = json
            })
        },
        toggleScrawl() {
            this.showScrawl = !this.showScrawl;
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/settings.scss';
    .film {
        margin: 10px 0;
        cursor: pointer;
        color: $light-gray;
        transition: color .3s;
        &:hover {
            color: $green;
        }


    }
    .scrawl-wrapper {
        opacity: 0;
        transition: opacity 2s;
        position: fixed;
        height: 100vh;
        width: 100vw;
        top: 0;
        left: 0;
        background-color: #000;
        z-index: -1;
        &.fade {
            opacity: 1;
            z-index: 1;
        }
    }
</style>
