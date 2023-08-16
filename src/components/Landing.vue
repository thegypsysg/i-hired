<template>
  <div>
    <div v-if="isLoading" class="text-center loading-page">
      <v-progress-circular :size="50" color="#fa2964" indeterminate />
    </div>
    <div v-if="!isLoading">
      <Banner :app-details="appDetails1" />
      <v-container>
        <TrendingApps
          :trending-card="categoryCard"
          :trending-btn="trendingBtn"
        />
      </v-container>
      <hr />
    </div>
  </div>
</template>

<script setup>
import Banner from '@/components/Banner.vue';
import TrendingApps from '@/components/TrendingApps.vue';
</script>

<script>
import axios from '@/util/axios';
export default {
  name: 'App',
  data() {
    return {
      drawer: false,
      isLoading: false,
      appDetails1: null,
      categoryCard: [],
      trendingBtn: [],
    };
  },

  mounted() {
    this.getAppDetails1();
    this.getCategoryCardData();
  },
  methods: {
    getAppDetails1() {
      this.isLoading = true;
      axios
        .get(`/app/details/${this.$appId}`)
        .then((response) => {
          const data = response.data.data;
          // console.log(data);
          this.appDetails1 = data;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
    getCategoryCardData() {
      this.isLoading = true;
      axios
        .get(`/categories/${this.$appId}`)
        .then((response) => {
          const data = response.data.data;
          // console.log(data);
          this.categoryCard = data.map((item, index) => {
            return {
              id: index + 1,
              img: this.$fileURL + item.image || '',
              desc: item.description || '',
              title: item.category_name || '',
              path: item.slug || '',
            };
          });
          this.trendingBtn = data.map((item) => {
            return {
              title: item.category_name || '',
              tag: item.category_name || '',
            };
          });
          // console.log(this.trendingCard);

          // app.config.globalProperties.$eventBus.$emit(
          //   'update-image',
          //   this.items
          // );
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
  },
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

.loading-page {
  height: 100vh;
  margin-top: 300px;
}

@media (max-width: 599px) {
  .loading-page {
    margin-top: 450px;
  }
}
</style>
