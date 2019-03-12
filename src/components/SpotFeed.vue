<template>
  <div class="spot-feed">
    <span v-if="is_playing">
      Currenty playing:
    </span>
    <span v-else>
      Recently played:
    </span>
    <br>
    {{ track_name }}
    by
    <a :href="track_link" target="_blank">
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
      oauth_token: 'BQCV1hYvVQSniKDs5XmNiBd8EE5ieYn3R_fWULtC__-RhzGF9CxmkedoZaWAxakwMcW1w6csUDqCax04cNFXm9Y9JdtpDWhKT4FHbqwduJP2dac-ipUeKF2EDs0FUgzI91uGaxSSM8NsHNpVoNimmSBO1NRD4Lo',
      url: 'https://api.spotify.com/v1/me',
      end_point: '/player',
      is_playing: '',
      artist_name: '',
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
        })
    }
  },
  mounted () {
    this.getAPI()
    // Removing temporarily
    //setInterval(function () {
      //this.getAPI()
    //}.bind(this), 30000);
  }
}
</script>

<style scoped>
  .spot-feed {
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
