<template>
  <div>
    <v-card-title>
      <vdiv>
        <p class="text-center">
          All Image List Updated By Admin
        </p>
      </vdiv>
    </v-card-title>
    <v-row>
      <v-col v-for="item in items" :key="item.id" cols="12" sm="6" md="4">
        <v-card class="mb-4">
          <v-img :src="item.image" height="200px">
            <v-img :src="'data:image/jpeg;base64,' + item.data" style="height: 200px;" />
            <v-expand-transition>
              <div v-if="item.showOverlay" class="overlay" />
            </v-expand-transition>
          </v-img>
          <v-card-title>{{ item.id }}</v-card-title>
          <v-card-text>{{ item.name }}</v-card-text>
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
      items: []
    }
  },
  methods: {
    async search (req) {
      try {
        await axios.get('http://localhost:4000/api/mydata', {
          params: {
            search: this.search || '' // eslint-disable-line
          }
        })
      } catch (error) {
        console.error(error)
      }
    },

    async fetchData () {
      try {
        const response = await axios.get('http://localhost:4000/api/mydata')
        if (response.status === 200) {
          console.log(response.data)
          this.items = response.data
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
        console.error(error)
      }
    }
  },
  mounted () {
    this.fetchData()
  }
}
</script>
