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
        <div class="field">
          <div class="buttons">
            <button @click="onPage(page - 1)" class="button" :disabled="page == 1">Prev</button>
            <button @click="onPage(page + 1)" class="button" :disabled="!(data.length > 0 && (total - (10 * page ) > 0))">Next</button>
          </div>
          <p>Total: {{ total }}</p>
          <p>Page: {{ page }}</p>
        </div>
      </div>
      <br>
      <div class="container">
        <template v-if="data.length > 0">
          <table class="table is-fullwidth">
            <thead>
              <tr>
                <th>Start</th>
                <th>End</th>
                <th>Text</th>
                <th>Action</th>
              </tr>
            </thead>
              <tbody>
                <tr v-for="(item, index) in data" v-bind:key="index">
                  <td>{{ item.start }}</td>
                  <td>{{ item.end }}</td>
                  <td v-html="item.text"></td>
                  <td><a :href="`${url}&t=${item.start}s`" target="_blank">Youtube</a></td>
                </tr>
            </tbody>
          </table>
        </template>
        <template v-else>
            <h1 style="text-align:center;font-size:2rem;">Data Empty</h1>
        </template>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import { debounce } from 'lodash'

export default {
  name: 'App',

  data() {
    return {
      url: 'https://www.youtube.com/watch?v=klnvttPfOUM',
      data: [],
      search: '',
      page: 1,
      total: 0
    }
  },

  methods: {
    fetchData() {
      axios.get('https://cari-teks-video-api.vercel.app/api/search', {
        params: {
          q: this.search,
          url: this.url,
          page: this.page
        }
      }).then(({ data }) => {
        this.data = data.data
        this.total = data.total
        this.page = data.page
      })
    },
    onPage(page) {
      this.page = page
      this.fetchData()
    }
  },

  watch: {
    search: debounce( function() {
      this.fetchData()
    }, 500)
  }

}
</script>
