<template>
  <div id="app">
    <div class="wrapper">
      <div class="select-container" >
        <select class="character-select" @change="handleChange" >
          <option value="" selected disabled >Select a Character</option>
          <option v-for="(character, key) in characters" :key="key" > 
              {{character.name}}
          </option>
        </select>
      </div>
      <div class="loading" v-if="loading" ><font-awesome-icon icon="spinner" spin ></font-awesome-icon> </div>
      <div class="info-container" :class="{hide: loading}" >
        <div class="info" v-if="displayInfo" >
          <h2>Character:</h2> 
          <div class="name">{{displayInfo.name}}</div>
          <h2>Films:</h2>
          <p>(click a film to see the scrawl! warning: background music autoplays)</p>
          <div class="films" >
            <film v-for="(film, key) in displayInfo.films" :key="key" :url="film" ></film>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import characterObj from './assets/characters';
import Film from './components/Film'

export default {
  name: 'app',
  components: {
    Film
  },
  data() {
    return {
      characters: characterObj.characters,
      displayInfo: null,
      loading: false
    }
  },
  methods: {
    handleChange(event) {
      console.log(event.target.selectedIndex)
      const index = event.target.selectedIndex - 1;
      const url = this.characters[index].url;
      this.getInfo(url, index)
    },
    getInfo(url, index) {
      this.loading = true;
      if (!url.includes("unknown")) {
        fetch(url)
          .then(response => {
            console.log(response)
              return response.json()
          }).then(json => {
            console.log("id:", json)
            this.displayInfo = json
            setTimeout(() => {
              this.loading = false;
            }, 1000)
          })
      } else {
        const name = this.characters[index].name;
        const string = `https://swapi.co/api/people/?search=${name}`
        const path = encodeURI(string)
        console.log(name, path)
        fetch(path)
          .then(resp => {
            return resp.json()
          })
          .then(json => {
            console.log("search:", json)
            this.displayInfo = json.results[0]
            setTimeout(() => {
              this.loading = false;
            }, 1000)
          })
      }
    }
  }
}
</script>

<style lang="scss" >
@import './assets/settings.scss';

  #app {
    background-color: $dark-gray;
    height: 100vh;
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;
    color: $light-gray;
    font-family: Arial;
  }

  .wrapper {
    width: 500px;
    position: relative;
    left: calc(50% - 250px);
    margin-top: 150px;
  }

  .info-container {
    opacity: 1;
    &.hide {
      opacity: 0;
    }
  }

  .select-container {
    position: relative;
    width: 200px;
      &::after {
      position: absolute;
      content: "";
      top: 16px;
      right: 10px;
      width: 0;
      height: 0;
      border: 6px solid transparent;
      border-color: $dark-gray transparent transparent transparent;
      z-index: 1;
    }
  }

  select.character-select {
    width: 200px;
    height: 40px;
    background-color: $white;
    border: 1px $light-gray solid;
    padding-left: 20px;
    font-size: 14px;
    appearance: none;
    border-radius: 8px;
    position: relative;
    margin-bottom: 16px;

  }

  .loading {
    position: relative;
    width: 200px;
    height: 200px;
    left: calc(50% - 100px);
    top: 100px;
    font-size: 50px;
    color: $light-gray;
    text-align: center;
  }

  .films {
    display: flex;
    flex-direction: row;
    flex-wrap:wrap;
  }

</style>
