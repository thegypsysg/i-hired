<template>
  <div class="bg-image" :class="{ 'bg-image-mobile': isSmall }">
    <v-container
      :class="{ 'banner-desktop': !isSmall, 'banner-mobile': isSmall }"
    >
      <v-row :class="{ 'd-flex justify-start': !isSmall }">
        <v-col :cols="!isSmall ? 6 : 12">
          <div
            class="hero_title"
            :data-aos="isSmall ? 'fade-left' : 'fade-right'"
            data-aos-offset="200"
            data-aos-duration="2000"
            data-aos-easing="ease-in-sine"
          >
            <h1 :style="{ 'font-size': !isSmall ? '96px' : '66px' }">
              Welcome !!!
            </h1>
            <h2
              class="mt-2"
              style="color: #fff; font-family: 'Oswald', sans-serif !important"
              :style="{ 'font-size': !isSmall ? '56px' : '46px' }"
            >
              WHATS YOUR BRAND. ?
            </h2>
            <!-- <h1
              style="color: rgb(255, 236, 75)"
              :style="{
                'font-size': !isSmall ? '55px' : '45px',
              }"
            >
              Looking for. ?
            </h1> -->
            <v-btn
              :height="isSmall ? 40 : 60"
              class="text-white elevation-2 mt-6"
              style="
                background-color: #ffa42e;
                border-color: #ffa42e;
                font-size: 20px;
                font-weight: 700;
              "
              elevation="2"
              @click="scrollToTrending"
            >
              <span class="mr-4">VIEW PRICE LIST</span>
            </v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <!--    <v-img class="bg-image" width="100%" src="@/assets/header.png" />-->
  </div>
</template>

<script>
import AOS from 'aos';

// onMounted(() => {
//   AOS.init();
// });

export default {
  // eslint-disable-next-line vue/multi-word-component-names, vue/no-reserved-component-names
  name: 'Banner',
  data() {
    return {
      drawer: false,
      items: [
        { title: 'Home', path: '/home', icon: 'home' },
        { title: 'Sign Up', path: '/signup', icon: 'face' },
        { title: 'Sign In', path: '/signin', icon: 'lock_open' },
      ],
      screenWidth: window.innerWidth,
    };
  },
  computed: {
    isSmall() {
      return this.screenWidth < 640;
    },
  },
  created() {
    window.addEventListener('resize', this.handleResize);
  },
  mounted() {
    AOS.init();
  },
  unmounted() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    handleResize() {
      this.screenWidth = window.innerWidth;
    },
    scrollToTrending() {
      const sectionElement = document.getElementById('trending');
      if (sectionElement && !this.isSmall) {
        const offset = 80; // Atur offset sesuai kebutuhan Anda
        const topPos =
          sectionElement.getBoundingClientRect().top +
          window.pageYOffset -
          offset;
        window.scrollTo({ top: topPos, behavior: 'smooth' });
      } else {
        const offset = 240; // Atur offset sesuai kebutuhan Anda
        const topPos =
          sectionElement.getBoundingClientRect().top +
          window.pageYOffset -
          offset;
        window.scrollTo({ top: topPos, behavior: 'smooth' });
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@400;500;600;700&display=swap');
.bg-image-mobile {
  height: 73vh !important;
}

.banner-desktop {
  height: calc(100vh - 90px);
  display: flex;
  align-items: center;
}
.banner-mobile {
  height: calc(80vh - 90px);
  display: flex;
  align-items: center;
}

.hero_title {
  text-shadow: 2px 2px 10px #000000;
  font-weight: 900;
}

.hero_title h1 {
  font-family: 'Oswald', sans-serif !important;
  color: #ffa42e;
}
</style>
