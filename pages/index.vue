<template>
  <div class="container">
    <div>
      <h1 class="title">Cats portfolio</h1>
      <div class=image-container>
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
    this.getRandomKitty();
  },
  methods: {
    getRandomKitty() {
      const url = "https://api.thecatapi.com/v1/images/search";
      this.makeRequest(url, "GET").then(result => {
        if (!result || !result[0] || !result[0].url) {
          throw new Error('Error: result has wrong format: ' + result.toString());
        }
        this.imgUrl = result[0].url;
      }).catch(e => {
        console.error(e);
      });
    },
    makeRequest(url, method = "GET") {
	    let request = new XMLHttpRequest();
      return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
          if (request.readyState !== 4) {
            return;
          }
          if (request.status >= 200 && request.status < 300) {
            resolve(request.response);
          } else {
            reject(request.statusText);
          }
        };
        request.responseType = "json";
        request.open(method, url, true);
        request.send();
      });
    },
  },
}
</script>

<style>
img {
  height: 500px;
  object-fit: contain;
  background-color: rgb(70, 70, 70);
}

.container {
  color: whitesmoke;
  background-color: rgb(42, 42, 42);
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.image-container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.image-container button {
  margin: 20px 0;
}

.image-container img {
  border: 1px solid whitesmoke;
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
  letter-spacing: 1px;
}
</style>
