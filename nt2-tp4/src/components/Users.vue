<template lang="html">
  <section class="src-components-users">
    <div class="jumbotron">
      <button class="btn btn-danger my-3 mr-3" @click="getUsersXHR">
        GET con XHR
      </button>
      <button class="btn btn-warning my-3 mr-3" @click="getUsersFetch">
        GET con Fetch
      </button>
      <button class="btn btn-success my-3 mr-3" @click="getUsersAXIOS">
        GET con AXIOS
      </button>
      <div v-if="users.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th v-for="(col, i) in getColumnas" :key="i">{{ col }}</th>
          </tr>
          <tr v-for="(user, i) in users" :key="i">
            <td v-for="(col, i) in getColumnas" :key="i">{{ user[col] }}</td>
          </tr>
        </table>
      </div>
    </div>
  </section>
</template>

<script lang="js">
export default {
  name: 'src-components-users',
  props: [],
  mounted() {
  },
  data() {
    return {
      url: 'https://60beb3776035840017c17899.mockapi.io/users',
      users: []
    }
  },
  methods: {
    getUsersXHR() {
      let xhr = new XMLHttpRequest
      xhr.open('GET', this.url)
      xhr.addEventListener('load', () => {
        if (xhr.status === 200) {
          let parsed = JSON.parse(xhr.response);
          this.users = parsed
        } else {
          console.error('Error en GET -> status: ' + status)
        }
      })
      xhr.addEventListener('error', e => {
        console.error('Error en XHR: ' + e)
      })
      xhr.send()
    },
    getUsersFetch() {
      fetch(this.url)
          .then(d => d.json())
          .then(r => {
            this.users = r
          })
          .catch(e => console.error(e))
    },
    getUsersAXIOS() {
      this.axios(this.url)
          .then(r => {
            this.users = r.data
          })
          .catch(e => console.error(e))
    }
  },
  computed: {
    getColumnas() {
      return Object.keys(this.users[0])
    }
  }
}
</script>

<style scoped lang="css">
.jumbotron {
  background: #b4ddb4;
  color: white;
}
</style>
