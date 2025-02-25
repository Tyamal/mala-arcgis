<template>
  <div class="landing-page">
    <h1>Welcome to Mala ArcGIS</h1>
    <p>Your one-stop solution for geographic data visualization.</p>
    <router-link to="/map">Go to Map</router-link>
  </div>
</template>

<script>
export default {
  name: 'LandingPage'
};
</script>

<style scoped>
.landing-page {
  text-align: center;
  margin-top: 50px;
}
</style>
