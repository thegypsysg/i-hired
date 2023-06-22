<template>
  <div class="bg-image" :class="{ 'bg-image-mobile': isSmall }">
    <v-container
      style="height: calc(100vh - 90px); display: flex; align-items: center"
    >
      <v-row :class="{ 'd-flex justify-start': !isSmall }">
        <v-col :cols="!isSmall ? 7 : 12">
          <div
            class="hero__text"
            :data-aos="!isSmall ? 'fade-right' : 'fade-left'"
            data-aos-offset="200"
            data-aos-duration="2000"
            data-aos-easing="ease-in-sine"
          >
            <h2
              style="color: #fff"
              :style="{ 'font-size': !isSmall ? '45px' : '35px' }"
            >
              Welcome !!!
            </h2>
            <h1
              class="mt-2"
              style="color: #fff"
              :style="{ 'font-size': !isSmall ? '55px' : '45px' }"
            >
              What Your Profession.?
            </h1>
            <v-btn
              :height="isSmall ? 40 : 60"
              class="text-white elevation-0 mt-6"
              style="
                background-color: rgb(253, 169, 0);
                border-color: rgb(253, 169, 0);
                font-size: 20px;
                font-weight: 700;
              "
              @click="scrollToTrending"
            >
              <span class="mr-4">View Jobs</span>
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
.bg-image-mobile {
  height: 73vh !important;
}
</style>
