<template>
  <div class="home">
    <img alt="Mining City" src="../assets/miningcity_logo.png" width="256px">
    <MiningCalculator />
    <footer class="footer mt-auto py-3">
      <div class="container">
        <span class="text-muted"><span class="text-danger">Disclosure:</span>Data are for giving information. Please verify data which are in site with other sites. <br/>Rate of Investment (ROI) : (Current value - Purchase price) / Purchase price</span>
        <p>Ver 0.1.2</p>
      </div>
    </footer>
  </div>

</template>

<script>
// @ is an alias to /src
import MiningCalculator from '@/components/MiningCalculator.vue'

export default {
  name: 'home',
  components: {
    MiningCalculator
  }
}
</script>
