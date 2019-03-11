<template>
  <div class="SpotFeed">
    <span v-if="is_playing">
      Currenty playing:
    </span>
    <span v-else>
      Recently played:
    </span>
    <br>
    {{ track_name }}
    by
    <a :href="`${track_link}`" target="_blank">
      {{ artist_name }}
    </a>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'SpotFeed',

  data () {
    return {
      oauth_token: 'BQDmrqTPVZnX36gll-BetXJN-9rtv4cEkIUOia2aM5kHo3efq6APCath13HyaGD--Rj1GXp6X5Y0NGQSDrWxHzY3B5AN1-spnUMXCPv68Hm7XcBOO8bII1EfBryrAHob8G0QMLcJgF4TYQSU6IzsGmsvi7uvpC0',
      url: 'https://api.spotify.com/v1/me',
      end_point: '/player',
      is_playing: '',
      artist_name:'',
      track_link: '',
      track_name: '',
      error: false
    }
  },
  methods: {
    getAPI () {
      axios
        .get(
          this.url + this.end_point,
          {
            headers: {
              Authorization: "Bearer " + this.oauth_token
            }
          }
        )
        .then(response => {
          this.player = response.data
          this.track = this.player.item

          this.is_playing = this.player.is_playing
          this.artist_name = this.track.artists[0].name
          this.track_link = this.track.artists[0].external_urls.spotify
          this.track_name = this.track.name

          console.log(this.player)
        })
    }
  },
  mounted () {
    this.getAPI()

    setInterval(function () {
      this.getAPI()
      console.log('refreshed')
    }.bind(this), 15000);
  }
}
</script>

<style scoped>
.SpotFeed {
  background: black;
  color: white;
  padding: 1em;
  position: fixed;
  right: 1em;
  top: 1em;
}

a {
  color: white;
}
</style>
