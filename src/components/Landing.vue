<template>
  <div>
    <Banner />
    <TrendingApps />
  </div>
</template>

<script setup>
import Banner from '@/components/Banner.vue';
import TrendingApps from './TrendingApps.vue';
</script>

<script>
export default {
  name: 'App',
  data() {
    return {
      drawer: false,
    };
  },
  components: { TrendingApps },
};
</script>

<style>
@font-face {
  font-family: 'Nunito';
  font-style: normal;
  font-weight: normal;
  src: url('@/assets/font/nunito/Nunito-VariableFont_wght.ttf')
    format('opentype');
}
</style>
