<template>
  <div class="spot-feed">
    <img :src="album_image" height="62" width="62"/>
    <div class="spot-feed__info">
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
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'SpotFeed',
  data () {
    return {
      //request: 'https://accounts.spotify.com/authorize?client_id=79f1ec5bf3904671b6b0a920a81961c8&redirect_uri=http:%2F%2Flocalhost:8080%2Fcallback&scope=user-read-recently-played%20user-modify-playback-state%20user-read-currently-playing&response_type=token&state=938402093',
      album_image:'',
      artist_name: '',
      end_point: '/player',
      is_playing: '',
      oauth_token: 'BQBuURN38cPtYpVG4izzj0xxwxYTfSPnBYdGaRL5oOAn36a6dYcIISmtT0Xeivca6onC0lzStAXUuVHj-3TKC-N3ZDjpuWw4JjWgwDlXxn1nMvA6_mbGEiC50upiFrstIuohqnEZBjX_u4jdxlCjJNDfCOe7uuyP_98u_RBvLP5XHGCIsWI',
      track_link: '',
      track_name: '',
      url: 'https://api.spotify.com/v1/me'
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

          this.album_image = this.player.item.album.images[2].url
          this.artist_name = this.track.artists[0].name
          this.track_link = this.track.artists[0].external_urls.spotify
          this.track_name = this.track.name
        })
    }
  },
  mounted () {
    this.getAPI()

    setInterval(function () {
      this.getAPI()
    }.bind(this), 90000);
  }
}
</script>

<style scoped>
  .spot-feed {
    background: black;
    color: white;
    display: flex;
    padding: 1em;
    position: fixed;
    right: 1em;
    top: 1em;
  }

  .spot-feed__info {
    margin-left: 1em;
  }

  button {
    display: block;
    width: 24%;
  }

  a {
    color: white;
  }
</style>
