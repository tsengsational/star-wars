<template>
    <div class="scrawl">
        <div class="fade"></div>
        <section class="star-wars">
            <div class="crawl">
                <div class="title">
                <p>Episode {{romanNum}}</p>
                <h1>{{film.title}}</h1>
                </div>
                <p>{{film.opening_crawl}}</p>
            </div>
        </section>
        <youtube class="vid" :player-vars="playerVars" :video-id="videoId" ref="youtube"></youtube>
    </div>
</template>

<script>
export default {
    props: ["film"],
    data() {
        return {
            videoId: "_D0ZQPqeJkk",
            playerVars: {
                autoplay: 1
            }
        }
    },
    created() {
    },
    computed: {
        romanNum() {
            const num = this.film.episode_id;
            return this.romanize(num);
        }
    },
    methods: {
        romanize(num) {
            var lookup = {M:1000,CM:900,D:500,CD:400,C:100,XC:90,L:50,XL:40,X:10,IX:9,V:5,IV:4,I:1},roman = '',i;
            for ( i in lookup ) {
                while ( num >= lookup[i] ) {
                roman += i;
                num -= lookup[i];
                }
            }
            return roman;
        }
    }

}
</script>

<style lang="scss" scoped>
    .scrawl {
    width: 100%;
    height: 100%;
    background: #000;
    overflow: hidden;
    position: fixed;
    top: 0;
    left: 0;
    }

    .vid {
        position: fixed;
        height: 100px;
        width: 100px;
        top: -200px;
    }

    .fade {
    position: relative;
    width: 100%;
    min-height: 60vh;
    top: -25px;
    background-image: linear-gradient(0deg, transparent, black 75%);
    z-index: 1;
    }

    .star-wars {
    display: flex;
    justify-content: center;
    position: relative;
    height: 800px;
    color: #feda4a;
    font-family: 'Pathway Gothic One', sans-serif;
    font-size: 500%;
    font-weight: 600;
    letter-spacing: 6px;
    line-height: 150%;
    perspective: 400px;
    text-align: justify;
    }

    .crawl {
    position: relative;
    top: 9999px;
    transform-origin: 50% 100%;
    animation: crawl 60s linear;
    }

    .crawl > .title {
    font-size: 90%;
    text-align: center;
    }

    .crawl > .title h1 {
    margin: 0 0 100px;
    text-transform: uppercase;
    }

    @keyframes crawl {
    0% {
        top: 500px;
        transform: rotateX(20deg)  translateZ(0);
    }
    100% { 
        top: -6000px;
        transform: rotateX(25deg) translateZ(-2500px);
    }
    }
</style>
