<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- erorrs -->
        <div class="error" v-if="error">
          <p> {{ error }} </p>
        </div>

        <!-- search -->
        <search
            :value="search"
            placeholder="Type username..."
            @search="search = $event"/>

        <!-- buttons -->
        <button v-if="!repos" class="btn btnPrimary" @click="getRepos">Search!</button>
        <button v-else class="btn btnPrimary" @click="getRepos">Search Again!</button>

        <!-- wrapper -->
        <div class="repos__wrapper" v-if="repos">
          <!-- item -->
          <div class="repos-item" v-for="repo in repos" :key="repo.id">
            <div class="repos-info">
              <a class="link" target="_blank" :href="repo.html_url">{{ repo.name }}</a>
              <span> {{ repo.stargazers_count }} ⭐ </span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import search from '@/components/Search.vue'
import axios from 'axios'

export default {
  components: { search },
  data () {
    return {
      search: '',
      error: null,
      repos: null
    }
  },
  methods: {
    getRepos () {
      axios
          .get(`https://api.github.com/users/${this.search}/repos`)
          .then(res => {
            this.error = null
            this.repos = res.data
          })
          .catch(err => {
            console.log(err)
            this.repos = null
            this.error = 'Can`t find this user'
          })
    }
  }
}
</script>

<style lang="scss" scoped>

.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

button {
  margin: 40px 0;
}

.repos__wrapper {
  width: 450px;
  margin: 30px 0;
}

.repos-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 20px 0;
  border-bottom: 1px solid #dbdbdb;
}

.error {
  margin-bottom: 10px;
  color: #ff0000;
}

</style>