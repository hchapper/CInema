<template>
  <div id="nav">
    <router-link to="/">Accueil</router-link>
    <router-link to="/recherche">Recherche</router-link>
  </div>
  <router-view/>
</template>
<script>


export default {
  name: 'App',
  methods: {
    getList() {
      this.axios.get(api).then((response) => {
        console.log(response.data)
      })
    }
  }
}
</script>

<style>

</style>
