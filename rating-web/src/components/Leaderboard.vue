
<template>
  <section>
    <div class="row at-row flex-center flex-middle">
      <div class="col-lg-24">
        <img class="super-justice" src="/static/img/justice.png">
        <h1 class="super-header">{{subtitle}}</h1>
      </div>
    </div>
    <div class="row at-row">
      <div class="col-lg-6" v-for="(item) in heroes">
        <div class="at-box-row">
          <at-card :bordered="false">
            <h4 slot="title" class="super-name">
              {{item.name}}
            </h4>
            <div>
              <img class="super-image" :src="item.img">
              <div class="super-rate-foot">
                X Votes
              </div>
            </div>
          </at-card>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      headerImage: "",
      subtitle:"",
      heroes: [],
      errors: []
    }
  },
    created() {
      axios.get(process.env.SITE)
        .then(response => {
          // console.log(`got the site info`)
          var page = response.data.pages.Leaderboard
          document.title = page.title
          this.headerImage = page.headerImage
          this.subtitle = page.subtitle
          return axios.get(process.env.API + "/heroes") 
        })
        .then(response => {
          this.heroes = response.data.payload
          this.$Notify({ title: 'Heroes loaded', message: 'Retrieved list of Heroes', type: 'success' })
        })
        .catch(e => {
          this.errors.push(e)
        })

    },
    methods: {
    }
  };
</script>