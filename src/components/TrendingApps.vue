<template>
  <v-container>
    <div
      id="trending-section"
      class="jumbotron-text"
      :class="{ 'mb-n4 mt-n10': isSmall }"
    >
      <h1
        align="center"
        class="header-title mb-n10"
        style="font-size: 56px; font-style: normal; font-weight: 700"
      >
        OUR BOOZE
      </h1>
      <div class="line-divider mt-10 mx-auto" />
      <!-- <p class="header-title-sub" style="margin-top: 32px">
        The gypsy Trending Web Apps
      </p> -->
    </div>
  </v-container>

  <v-container id="trending" class="wrapper-box">
    <template v-if="isSmall">
      <v-row class="trending__app__wrapper">
        <transition-group name="card-transition" mode="out-in">
          <v-col
            v-for="(card, i) in filteredItemsMobile"
            :key="i"
            xs="6"
            sm="6"
            md="4"
            cols="12"
          >
            <v-lazy :options="{ threshold: 0.5 }" min-height="200">
              <div class="trending__app d-flex justify-center mb-8">
                <div class="title-card title-card-mobile mx-auto">
                  <h1>{{ card.title }}</h1>
                </div>
                <v-card
                  style="
                    box-shadow: 0px 5px 25px rgba(0, 0, 0, 0.15);
                    border-radius: 0px;
                  "
                >
                  <div class="img-cont">
                    <div class="cart clearfix animate-effect">
                      <div class="action">
                        <div class="card-desc-cont-mobile px-2 text-center">
                          <h2>{{ card.title }}</h2>
                          <p>{{ card.desc }}</p>
                          <v-btn
                            elevation="4"
                            style="
                              background-color: #ffa42e;
                              border-radius: 0;
                              padding-left: 6px;
                              padding-right: 6px;
                              padding-top: 4px;
                              padding-bottom: 4px;
                              font-weight: 600;
                              font-size: 12px;
                            "
                          >
                            <span class="text-black" style="">VIEW PRICES</span>
                          </v-btn>
                        </div>
                      </div>
                    </div>
                    <div class="overlay"></div>
                    <v-img
                      :src="card.img"
                      gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                      height="200px"
                      cover
                      class="img-item"
                    >
                      <template #placeholder>
                        <div class="skeleton" />
                      </template>
                    </v-img>
                  </div>
                </v-card>
              </div>
            </v-lazy>
          </v-col>
        </transition-group>
      </v-row>
    </template>
    <template v-if="!isSmall">
      <v-row class="trending__app__wrapper">
        <transition-group name="card-transition" mode="out-in">
          <v-col
            v-for="(card, i) in filteredItemsDesktop"
            :key="i"
            xs="6"
            sm="6"
            md="4"
            cols="12"
            class="card"
          >
            <v-lazy :options="{ threshold: 0.5 }" min-height="200">
              <div class="trending__app d-flex justify-center mb-8">
                <div class="title-card mx-auto">
                  <h1>{{ card.title }}</h1>
                </div>
                <v-card
                  style="
                    box-shadow: 0px 5px 25px rgba(0, 0, 0, 0.15);
                    border-radius: 10px;
                  "
                >
                  <div class="img-cont">
                    <div class="cart clearfix animate-effect">
                      <div class="action">
                        <div class="card-desc-cont px-2 text-center">
                          <h2>{{ card.title }}</h2>
                          <p>{{ card.desc }}</p>
                          <v-btn
                            elevation="4"
                            style="
                              background-color: #ffa42e;
                              border-radius: 0;
                              padding-left: 16px;
                              padding-right: 16px;
                              padding-top: 10px;
                              padding-bottom: 10px;
                            "
                          >
                            <span class="text-black" style="">VIEW PRICES</span>
                          </v-btn>
                        </div>
                      </div>
                    </div>
                    <div class="overlay"></div>
                    <v-img
                      :src="card.img"
                      gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                      cover
                      class="img-item"
                    >
                      <template #placeholder>
                        <div class="skeleton" />
                      </template>
                    </v-img>
                  </div>
                </v-card>
              </div>
            </v-lazy>
          </v-col>
        </transition-group>
      </v-row>
    </template>
    <div
      class="card-footer d-flex justify-center align-center"
      :class="{ 'mt-16 mb-10': !isSmall, 'my-4': isSmall }"
      style="gap: 10px"
    >
      <!-- <div class="line-divider" /> -->
      <v-btn
        class="btn-section btn-primary v-btn v-btn--has-bg theme--light elevation-0 text-white d-flex align-center py-8 px-16"
        :class="{ 'btn-section-2': isSmall }"
      >
        <span>VIEW ALL OUR BOOZE</span>
      </v-btn>
      <!-- <div class="line-divider" /> -->
    </div>
  </v-container>
</template>

<script>
import app from '@/util/eventBus';
// import { computed, onMounted, onUnmounted } from "vue";
// import eventBus from "@/util/eventBus";
import { mapState } from 'vuex';

export default {
  name: 'TrendingApps',
  data() {
    return {
      selectedTag: null,
      // activeTagHeader: null,
      // trendingBtn: [
      //   {
      //     title: "View All",
      //   },
      //   { title: "Promo App", tag: "Promo App" },
      //   { title: "Alcohol App", tag: "Alcohol App" },
      //   { title: "Jobs App", tag: "Job App" },
      //   { title: "On The Run Apps", tag: "On the Run App" },
      //   { title: "Housing App", tag: "Housing App" },
      //   { title: "Travel App", tag: "Travel App" },
      //   { title: "Staycation App", tag: "Staycation App" },
      //   { title: "Listings App", tag: "Listing App" },
      //   { title: "Tournaments App", tag: "Tournament App" },
      //   { title: "Cafe App", tag: "Cafe App" },
      //   { title: "Overseas Study App", tag: "Overseas Study App" },
      // ],
      trendingCard: [
        {
          img: require('@/assets/image/single.jpg'),
          title: 'Single Malt',
          desc: 'Singleton, Talisker, Glenlivet, Laphroaig, Glenfarclas and More...',
        },
        {
          img: require('@/assets/image/whiskeybanner.jpg'),
          title: 'Whisky',
          desc: 'Chivas Regal , Monkey Shoulder, Johnnie Walker, Glenfiddich and more ....',
        },
        {
          img: require('@/assets/image/gin.jpg'),
          title: 'Gin',
          desc: "Roku, Hendrick's , Gordon's , Beefeater, Bombay Sapphire, Tanqueray and More...",
        },
        {
          img: require('@/assets/image/single.jpg'),
          title: 'Single Malt',
          desc: 'Singleton, Talisker, Glenlivet, Laphroaig, Glenfarclas and More...',
        },
        {
          img: require('@/assets/image/whiskeybanner.jpg'),
          title: 'Whisky',
          desc: 'Chivas Regal , Monkey Shoulder, Johnnie Walker, Glenfiddich and more ....',
        },
        {
          img: require('@/assets/image/gin.jpg'),
          title: 'Gin',
          desc: "Roku, Hendrick's , Gordon's , Beefeater, Bombay Sapphire, Tanqueray and More...",
        },
      ],
      // filteredCards: [],
      selectedType: 0,
      activeIndex: 1,
      screenWidth: window.innerWidth,
    };
  },
  computed: {
    ...mapState(['activeTag']),

    trendingBtn() {
      return [
        { title: 'Promo App', tag: 'Promo App' },
        { title: 'Alcohol App', tag: 'Alcohol App' },
        { title: 'Jobs App', tag: 'Job App' },
        { title: 'On The Run Apps', tag: 'On the Run App' },
        { title: 'Housing App', tag: 'Housing App' },
        { title: 'Travel App', tag: 'Travel App' },
        { title: 'Staycation App', tag: 'Staycation App' },
        { title: 'Listings App', tag: 'Listing App' },
        { title: 'Tournaments App', tag: 'Tournament App' },
        { title: 'Cafe App', tag: 'Cafe App' },
        { title: 'Overseas Study App', tag: 'Overseas Study App' },
      ];
    },
    isSmall() {
      return this.screenWidth < 640;
    },
    filteredItemsMobile() {
      // console.log(this.activeTag);
      if (!this.activeTag || this.activeTag == undefined) {
        return this.trendingCard;
      } else {
        // const searchTextLower = this.search.toLowerCase();
        return this.trendingCard.filter((item) => {
          return item.title.includes(this.activeTag);
        });
      }
    },
    filteredItemsDesktop() {
      // console.log(this.selectedTag);
      if (!this.selectedTag) {
        return this.trendingCard;
      } else {
        // const searchTextLower = this.search.toLowerCase();
        return this.trendingCard.filter((item) => {
          return item.title.includes(this.selectedTag);
        });
      }
    },
  },
  created() {
    window.addEventListener('resize', this.handleResize);
  },
  mounted() {
    app.config.globalProperties.$eventBus.$on(
      'scrollToCardSection',
      this.scrollToCardSection
    );
  },
  beforeUnmount() {
    app.config.globalProperties.$eventBus.$off(
      'scrollToCardSection',
      this.scrollToCardSection
    );
    // eventBus.off("filter-card-header", this.filterCards);
  },
  unmounted() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    // selectTag(tag) {
    //   this.activeTag = tag; // Menetapkan tag yang dipilih sebagai tag aktif di komponen kartu
    // },
    scrollToCardSection() {
      const cardSection = document.getElementById('trending');
      const cardRect = cardSection.getBoundingClientRect();
      const scrollTop =
        window.pageYOffset || document.documentElement.scrollTop;
      const offset = cardRect.top + scrollTop - 350; // Nilai offset yang diinginkan, dalam piksel

      window.scrollTo({
        top: offset,
        behavior: 'smooth',
      });
      // window.scrollBy(0, -scrollOffset);
    },

    selectTag(tag) {
      this.$store.commit('setActiveTag', tag); // Menetapkan tag yang dipilih sebagai tag aktif
    },
    filterCards(tag) {
      // console.log("ok");
      this.selectedTag = tag;
    },
    filterCardHeader(tag) {
      this.activeTag = tag;
      // console.log(this.activeTag);
    },
    countCards(tag) {
      const count = this.trendingCard.filter(
        (trend) => trend.tag === tag
      ).length;
      return count;
    },
    handleResize() {
      this.screenWidth = window.innerWidth;
    },
    previousSlide() {
      this.activeIndex--;
    },
    nextSlide() {
      this.activeIndex++;
    },
  },
};
</script>

<style scoped>
.line-divider {
  height: 2px;
  width: 150px;
  background: #ffa42e;
}

.title-card {
  position: absolute;
  top: -20px;
  width: 80%;
  color: white;
  background-color: #ffa42e;
  padding: 10px auto;
  z-index: 100;
  display: flex;
  justify-content: center;
  margin: 0 auto;
  border-radius: 30px;
}
.title-card-mobile {
  width: 90%;
  font-size: 12px !important;
}

.card-desc-cont h2 {
  color: white;
}
.card-desc-cont p {
  color: white;
  margin-bottom: 10px;
}
.card-desc-cont-mobile h2 {
  font-size: 16px;
  color: white;
}
.card-desc-cont-mobile p {
  font-size: 12px;
  color: white;
  margin-bottom: 10px;
}
.my-slide {
  position: relative !important;
}
.view-all {
  background: #0596d5;
  color: white;
  height: 50px !important;
  z-index: 100 !important;
  /* Gaya view all yang sticky */
}
.card-transition-enter-active,
.card-transition-leave-active {
  transition: transform 0.5s, opacity 0.3s;
}

.card-transition-enter {
  opacity: 0;
  transform: translateX(-50%);
}

.card-transition-leave-to {
  opacity: 0;
  transform: scale(0.8);
}

.img-item {
  transition: all 0.3s;
  width: 100%;
  height: auto;
  transform: scale(1);
}

.trending__app {
  position: relative;
}
.trending__app:hover .title-card {
  display: none;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: 100;
}

.trending__app:hover .overlay {
  opacity: 1;
}

.skeleton {
  width: 100%;
  height: 100%;
  border-radius: 0;

  background: linear-gradient(-90deg, #f2f2f2 0%, #e1e1e1 50%, #f2f2f2 100%);
  background-size: 400% 400%;
  animation: skeleton 1.6s ease infinite;
}

.desktop-card-desc {
  height: 70px !important;
}

.btn-section {
  background-color: #ffa42e;
  border-color: #ffa42e;
  font-weight: 400;
  font-size: 20px;
  border-radius: 50px;
}
.btn-section-2 {
  font-size: 14px;
}

@keyframes skeleton {
  0% {
    background-position: 100% 0;
  }
  100% {
    background-position: -100% 0;
  }
}
</style>
