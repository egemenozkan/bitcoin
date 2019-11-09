<template>
  <div class="home">
    <img alt="Mining City" src="../assets/miningcity_logo.png" width="256px">
    <MiningCalculator />
    <footer class="footer mt-auto py-3">
      <div class="container">
        <span class="text-muted"><span class="text-danger">Disclosure:</span>Data are for giving information. Please verify data which is in site with other sites.</span>
        <p>Ver 0.1</p>
      </div>
    </footer>
    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=UA-151021567-2"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());

      gtag('config', 'UA-151021567-2');
    </script>

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
