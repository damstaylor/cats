<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        cats
      </h1>
      <div style="display: flex; align-items: center; justify-content: center; flex-direction: column;">
        <button type=button @click=getRandomKitty>Get random kitty!</button>
        <img :src="imgUrl">
      </div>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          rel="noopener noreferrer"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgUrl: null,
    };
  },
  created() {
  },
  methods: {
    get(url) {
      const _com = this;
      let xhr = new XMLHttpRequest();
      xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
          const JSON_DATA = JSON.parse(this.responseText);
          _com.imgUrl = JSON_DATA[0].url;
        }
      };
      xhr.open("GET", url, true);
      xhr.send();
    },
    getRandomKitty() {
      const url = "https://api.thecatapi.com/v1/images/search";
      this.get(url);
    },
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
