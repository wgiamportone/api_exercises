<template>
  <div class="insta-feed">
    <h1>{{ full_name }}</h1>
    <h2><a :href="`${user_link}`" target="_blank">@{{ username }}</a></h2>
    <br><br>
    <h3>Most recent:</h3>
    <nav>
      <button @click.prevent="setActiveTag()">
        all
      </button>
      <button v-for="tag in tags" @click.prevent="setActiveTag(tag)">
        {{ tag }}
      </button>
    </nav>
    <ul>
      <InstaItem
        v-for="item in feed"
        :data="item"
        :activeTag="activeTag"
      />
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
import InstaItem from './InstaItem.vue'

export default {
  name: 'InstaFeed',
  components: {
    InstaItem,
  },
  data () {
    return {
      access_token: '?access_token=3153341.f24bbb3.e22b113ec77548fbb0b17ec9db5ec6bc',
      url: 'https://api.instagram.com/v1',
      recent: '/users/self/media/recent/',
      feed: [],
      tags: [],
      activeTag: '',
      full_name: '',
      username: '',
      user_link: '',
      next_url: '',
      error: false
    }
  },
  methods: {
    getAPI () {
      axios
        .get(this.url + this.recent + this.access_token)
        .then(response => {
          this.feed = response.data.data
          this.full_name = this.feed[0].user.full_name
          this.username = this.feed[0].user.username
          this.user_link = 'https://www.instagram.com/' + this.username
          this.tags = this.makeNav()
        })
    },
    makeNav () {
      var tempArr = []

      this.feed.forEach((item) => {
        if (item.tags.length) {
          item.tags.forEach((tag) => {
            if (tempArr.indexOf(tag) == -1) {
              tempArr.push(tag)
            }
          })
        }
      })

      return tempArr.sort()
    },

    setActiveTag (tag) {
      this.activeTag = tag
    }
  },
  mounted () {
    this.getAPI()
  }
}
</script>

<style scoped>
  nav,
  ul {
    align-content: space-between;
    display: flex;
    flex-wrap: wrap;
  }

  nav {
    margin-bottom: 18px;
  }

  button {
    margin: 6px;
  }

  ul,
  li {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  li {
    padding: 3px 6px;
    flex-basis: 30px;
  }

  img {
    display: block;
    height: auto;
    max-width: 100%;
  }
</style>
