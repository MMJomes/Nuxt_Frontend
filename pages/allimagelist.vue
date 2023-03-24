<template>
  <div>
    <div class="d-flex justify-center">
      <v-card-title>
      All Image List
      </v-card-title>
    </div>
    <v-row>
      <v-col v-for="item in items" :key="item.id" cols="12" sm="6" md="4">
        <v-card class="mb-4">
          <v-img :src="item.image" height="200px">
            <v-img :src="'data:image/jpeg;base64,' + item.data" style="height: 200px;" />
            <v-expand-transition>
              <div v-if="item.showOverlay" class="overlay" />
            </v-expand-transition>
          </v-img>
          <v-card-title><span style="color: aqua;"> Image Name</span>  :{{ item.name }}</v-card-title>
          <v-card-subtitle><span style="color: black;">Updated At: {{ uploaded_at }} </span></v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>

import axios from 'axios'

export default {

  data () {
    return {
      imageuploadtime: null,
      second: null,
      minutes: null,
      hours: null,
      days: null,
      uploaded_at: null,
      items: []
    }
  },
  methods: {
    async fetchData () {
      try {
        const response = await axios.get('http://localhost:4000/api/mydata')
        console.log(response.data)
        if (response.status === 200) {
          this.items = response.data
          this.imageuploadtime = Date.now() - response.data[0].id
          this.second = Math.floor(this.imageuploadtime / 1000)
          this.minutes = Math.floor(this.second / 60)
          this.hours = Math.floor(this.minutes / 60)
          this.days = Math.floor(this.hours / 24)
          this.uploaded_at = ` ${this.days} days, ${this.hours % 24} hours, ${this.minutes % 60} minutes, and ${this.second % 60} seconds.`

          // console.log(this.imageuploadtime)
        } else {
          console.log(response.data)
          this.items = []
          this.items.push({
            id: 0,
            name: 'No Data',
            image: 'https://cdn.vuetifyjs.com/images/cards/house.jpg',
            data: '',
            showOverlay: false
          })
        }
      } catch (error) {
        console.log('ED')
        console.error(error)
      }
    }
  },
  mounted () {
    this.fetchData()
  }
}
</script>
<style>
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.v-card:hover .overlay {
  opacity: 1;
}
</style>
