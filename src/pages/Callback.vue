<template>
  <div>
    <h1>Loading...</h1>
  </div>
</template>

<script>
import axios from 'axios'
const qs = require('qs');

export default {
  name: 'Callback',
  created() {
    if (this.$route.query.code) {
      const spotifyCode = this.$route.query.code
      const basicAuth = `Basic ${(Buffer.from(`${process.env.VUE_APP_CLIENT_ID}:${process.env.VUE_APP_CLIENT_SECRET}`).toString('base64'))}`
      return axios.post('https://accounts.spotify.com/api/token', 
        new URLSearchParams({ 
          code: spotifyCode, 
          grant_type: 'authorization_code', 
          redirect_uri: process.env.VUE_APP_REDIRECT_URI 
        }).toString(), 
        { 
          headers: { 
            Authorization: basicAuth, 
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        })
        .then(response => {
          sessionStorage.setItem('SPOTIFY_ACCESS_TOKEN', response.data.access_token);
          return this.$router.push('Playlists')
        })

    }
  }
}
</script>

<style scoped>
* {
  background-color: black;
  font-family: "Poppins", sans-serif;
  align-content: center;
}
h1{
  font-size: 50px;
}
</style>