<template>
  <div class="container">
    <div class="random-kitty">
      <div class=image-container>
        <img :src="imgUrl">
        <button type=button @click=getRandomKitty>Get random kitty!</button>
      </div>
      <h1 class="title">Cats portfolio</h1>
    </div>
    <div class=pictures>
      <div v-for="(pic, idx) in pictures" :key=idx class=image-container>
        <img :src="pic.url">
      </div>
    </div>
    <div></div>
    <footer>
      <nav>
        <ul class="pagination">
            <li><a @click="setPage(0)">«</a></li>
            <li><a @click="decreasePage">&lt;</a></li>
            <li v-show="page > 1"><span>...</span></li>
            <li v-show="page > 0"><a @click="decreasePage">{{ page }}</a></li>
            <li class="current-page-indicator"><a>{{ page + 1 }}</a></li>
            <li v-show="getNumberOfPages && page < getNumberOfPages - 1"><a @click="increasePage">{{ page + 2 }}</a></li>
            <li v-show="getNumberOfPages && page < getNumberOfPages - 2"><span>...</span></li>
            <li v-show="getNumberOfPages && page < getNumberOfPages - 1"><a @click="increasePage">&gt;</a></li>
            <li><a @click="setPage(getNumberOfPages - 1)">»</a></li>
        </ul>
      </nav>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      API_KEY: "761e080c-3f90-4fc2-bfb5-bebf6a9c1c16",
      headers: null,
      imgUrl: null,
      fetchedData: null,
      pictures: [],
      limit: 9,
      page: 0,
      order: "Desc",
    };
  },
  mounted() {
    this.getRandomKitty();
    this.getPictures();
  },
  computed: {
    getResponseHeaders() {
      if (!this.headers) {
        return null;
      }
      let obj = {};
      const headersKeyValue = this.headers.split('\n');
      headersKeyValue.filter(str => str !== "").forEach(str => {
        const arr = str.split(': ');
        obj[arr[0]] = arr[1];
      });
      return obj;
    },
    getPaginationCount() {
      return this.getResponseHeaders ? this.getResponseHeaders['pagination-count'] : 0;
    },
   
    getNumberOfPages() {
      return Math.ceil(this.getPaginationCount / this.limit);
    },
  },
  methods: {
    getPictures() {
      const url = `https://api.thecatapi.com/v1/images/search?limit=${this.limit}&page=${this.page}&order=${this.order}`;
      this.makeRequest(url, "GET").then(result => {
        this.headers = result.headers;
        const response = result.response;
        if (!Array.isArray(response)) {
          throw new Error('Error: response has wrong format: ' + response.toString());
        }
        console.log(result);
        this.pictures = response;
      }).catch(e => {
        console.error(e);
      });
    },
    getRandomKitty() {
      const url = "https://api.thecatapi.com/v1/images/search";
      this.makeRequest(url, "GET").then(result => {
        result = result.response;
        if (!result || !result[0] || !result[0].url) {
          throw new Error('Error: result has wrong format: ' + result.toString());
        }
        this.imgUrl = result[0].url;
      }).catch(e => {
        console.error(e);
      });
    },
    makeRequest(url, method = "GET") {
      const _app = this;
	    let request = new XMLHttpRequest();
      return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
          if (request.readyState !== 4) {
            return;
          }
          if (request.status >= 200 && request.status < 300) {
            resolve({headers: request.getAllResponseHeaders(), response: request.response});
          } else {
            reject(request.statusText);
          }
        };
        request.responseType = "json";
        request.open(method, url, true);
        request.setRequestHeader('x-api-key', _app.API_KEY);
        request.send();
      });
    },
    setPage(pageIdx = 0) {
      if (pageIdx < 0) {
        pageIdx = 0;
      }
      this.page = pageIdx;
    },
    increasePage() {
      if (this.page < this.getNumberOfPages - 1) {
        this.page++;
      }
    },
    decreasePage() {
      if (this.page > 0) {
        this.page--;
      }
    },
  },
  watch: {
    page: function() {
      this.getPictures();
    }
  },
}
</script>

<style>
a:hover {
  cursor: pointer;
}

img {
  height: 200px;
  object-fit: contain;
  background-color: rgb(70, 70, 70);
}

.pictures {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
}

.pictures .image-container {
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
}

.pictures .image-container img {
  height: 150px;
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

.container .random-kitty .image-container {
  padding: 20px;
}

.image-container {
  display: flex;
  align-items: center;
  flex-direction: column;
  background-color: #555555;
}

.image-container button {
  margin: 10px 0;
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
  margin-bottom: 20px;
}

footer {
  margin: 10px 0;
}

nav {
  display: flex;
  justify-content: center;
}
        
.pagination {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  column-gap: 5px;
}
        
.pagination li {
  display: flex;
  align-items: center;
  margin: 0 1px;
}

.pagination li.current-page-indicator {
  border: 1px solid whitesmoke;
  border-radius: 10%;
  justify-content: center;
}
</style>
