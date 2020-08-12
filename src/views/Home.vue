<template>
  <div class="home">
    <HeaderZeeb/>

    <HeroZeeb/>

    <WomanZeeb/>

    <InstaZeeb/>

    <PartnerZeeb/>

    <FooterZeeb/>
  </div>
</template>

<script>
import HeaderZeeb from "@/components/HeaderZeeb.vue";
import HeroZeeb from "@/components/HeroZeeb.vue";
import WomanZeeb from "@/components/WomanZeeb.vue";
import InstaZeeb from "@/components/InstaZeeb.vue";
import PartnerZeeb from "@/components/PartnerZeeb.vue";
import FooterZeeb from "@/components/FooterZeeb.vue";




  // @ is an alias to /src

  export default {
    name: 'Home',
    components: {
      HeaderZeeb,
      HeroZeeb,
      WomanZeeb,
      InstaZeeb,
      PartnerZeeb,
      FooterZeeb
    }
  }
</script>