<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">
        <v-img :src="'data:image/jpeg;base64,' + item.data" style="width: 200px; height: 200px;" />
      </li>
    </ul>
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
    async fetchData () {
      try {
        const response = await axios.get('http://localhost:4000/api/mydata')
        this.items = response.data
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
