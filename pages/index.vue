<template>
  <div class="container">
    <div>
      <h1 class="title">Cats portfolio</h1>
      <div class=random-image>
        <button type=button @click=getRandomKitty>Get random kitty!</button>
        <img :src="imgUrl">
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
img {
  height: 500px;
  width: 100%;
  object-fit: contain;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.random-image {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.random-image button {
  margin: 20px 0;
}

.random-image img {
  border: 1px solid grey;
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
</style>
