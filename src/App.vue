<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <div class="field">
          <label for="url">Url youtube</label>
          <div class="control">
            <input v-model="url" type="url" class="input" placeholder="url...">
          </div>
        </div>
        <div class="field">
          <label for="search">Search</label>
          <div class="control">
            <input v-model="search" type="search" class="input" placeholder="search...">
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'

export default {
  name: 'App',

  data() {
    return {
      url: 'https://www.youtube.com/watch?v=klnvttPfOUM',
      search: '',
      data: []
    }
  },

  methods: {
    fetchData() {
      axios.get('https://cari-teks-video-api.vercel.app/api/search', {
        params: {
          q: this.search,
          url: this.url,
        }
      }).then(({ data }) => {
        console.log(data)
      })
    }
  },

  watch: {
    search: _.debounce( function() {
      this.fetchData();
    }, 500)
  }

}
</script>
