<template>
  <div>
    <Nuxt />
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Quis consequuntur dicta alias vel quidem ipsam. Quibusdam similique aut fugit iure quidem officiis error facere. Corrupti explicabo nam ea atque eius?
    </p>
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
