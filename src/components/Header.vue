<template>
  <v-app-bar
    :class="{ 'app-bar-mobile': isSmall }"
    color="white"
    elevation="1"
    fixed
  >
    <router-link to="/">
      <div class="logo-img-container d-flex align-center">
        <v-img
          class="logo-img"
          :src="$fileURL + logo"
          height="40"
          :class="{ 'ml-8': isWelcome }"
        >
          <template #placeholder>
            <div class="skeleton" />
          </template>
        </v-img>
      </div>
    </router-link>
    <div v-if="isWelcome" class="ml-10 d-flex flex-row header-info">
      <div v-if="!isSmall" class="divider" />
      <span :class="{ 'header-info-span': isSmall }">Sign Up / Login</span>
    </div>
    <v-spacer v-if="isWelcome" />
    <form v-if="!isWelcome" class="navbar__search navbar__search__desktop">
      <input
        id="product_name"
        class="form-control mr-sm-2"
        type="text"
        placeholder="Type anything that you are looking for "
        aria-label="Search"
        data-autocompleturl="https://boozards.com/merchant-product/search"
      />
      <button class="btn btn--search" type="submit">
        <v-icon color="white"> mdi-magnify </v-icon>
      </button>
    </form>
    <div v-if="!isWelcome" class="desktop__app">
      <v-menu>
        <template #activator="{ props }">
          <v-btn
            style="
              margin-left: 30px;
              margin-right: 30px;
              font-size: 16px;
              color: #494949;
            "
            v-bind="props"
            variant="text"
          >
            {{ itemSelected }}
            <v-icon right dark> mdi-menu-down </v-icon>
          </v-btn>
        </template>
        <!-- <v-list>
          <v-list-item
            v-for="(item, index) in country"
            :key="index"
            :value="index"
            @click="changeItemSelected(item.title)"
          >
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list> -->
      </v-menu>
    </div>
    <v-btn v-if="!isWelcome" elevation="0" class="btn_sign__up" to="/welcome">
      Sign up / Register
    </v-btn>
    <v-btn v-if="!isWelcome" icon @click="drawer = !drawer">
      <v-img
        src="@/assets/images/icons/user_icon.png"
        style="height: 48px; width: auto"
      />
    </v-btn>

    <template v-if="!isWelcome" #extension>
      <div
        class="mobile__app text-center scroll-container d-flex flex-column justify-center align-content-space-between mx-2"
      >
        <div>
          <v-menu>
            <template #activator="{ props }">
              <v-btn
                style="
                  margin-left: 30px;
                  margin-right: 30px;
                  font-size: 16px;
                  color: #494949;
                "
                v-bind="props"
                variant="text"
              >
                {{ itemSelected }}
                <v-icon right dark> mdi-menu-down </v-icon>
              </v-btn>
            </template>
            <!-- <v-list>
              <v-list-item
                v-for="(item, index) in country"
                :key="index"
                :value="index"
                @click="changeItemSelected(item.title)"
              >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list> -->
          </v-menu>
        </div>
        <form
          class="navbar__search navbar__search__mobile mx-auto"
          @submit="preventSubmit"
        >
          <input
            id="product_name"
            class="form-control mr-sm-2"
            type="text"
            placeholder="Type anything that you are looking for "
            aria-label="Search"
            data-autocompleturl="https://boozards.com/merchant-product/search"
          />
          <button class="btn btn--search" type="submit">
            <v-icon color="white"> mdi-magnify </v-icon>
          </button>
        </form>

        <div class="my-slide d-flex">
          <v-btn
            class="sub-menu-btn view-all"
            :class="{
              active: isSelected,
            }"
            style="box-shadow: 0 5px 25px rgba(0, 0, 0, 0)"
            @click="selectTag('')"
          >
            <p style="font-size: 12px" elevation>View All</p>
            <!-- <span class="badge" :class="isSelected ? 'active' : ''"
            >2.7K</span
          > -->
          </v-btn>
          <v-slide-group v-model="activeTag">
            <v-slide-group-item
              v-for="btn in trendingBtn"
              :key="btn.tag"
              v-slot="{ isSelected }"
              :value="btn.tag"
            >
              <v-btn
                class="sub-menu-btn"
                :class="{
                  active: isSelected,
                }"
                style="box-shadow: 0 5px 25px rgba(0, 0, 0, 0)"
                @click="selectTag(btn.tag)"
              >
                <p style="font-size: 12px" elevation>
                  {{ btn.title }}
                  <span>{{
                    countCards(btn.tag) == 0 ? '' : `(${countCards(btn.tag)})`
                  }}</span>
                </p>
                <!-- <span class="badge" :class="isSelected ? 'active' : ''"
                >2.7K</span
              > -->
              </v-btn>
            </v-slide-group-item>
          </v-slide-group>
        </div>
      </div>
    </template>
  </v-app-bar>
  <v-navigation-drawer
    v-if="!isWelcome || (isWelcome && isSmall)"
    v-model="drawer"
    temporary
    location="right"
  >
    <div class="drawer__top">
      <a style="font-size: 1.125rem; color: white">Sign up / Register</a>
    </div>
    <ul class="v-list-cont d-flex mt-4" nav dense>
      <li class="v-list-item">
        <v-list-item-title>Home</v-list-item-title>
      </li>

      <li class="v-list-item">
        <v-list-item-title>Employer</v-list-item-title>
      </li>

      <li class="v-list-item">
        <v-list-item-title>Agency</v-list-item-title>
      </li>
    </ul>
  </v-navigation-drawer>
</template>

<script>
import { mapState, mapMutations } from 'vuex';
import app from '@/util/eventBus';
import axios from '@/util/axios';

// import eventBus from "@/util/eventBus";
// import eventBus from "@/util/eventBus";

export default {
  // eslint-disable-next-line vue/multi-word-component-names, vue/no-reserved-component-names
  name: 'Header',
  props: ['isWelcome'],
  data() {
    return {
      trendingBtn: [],
      drawer: false,
      logo: '',
      country: [
        { title: 'Singapore', path: '#' },
        { title: 'Mumbai', path: '#' },
        { title: 'Goa', path: '#' },
        { title: 'Kuala Lumpur', path: '#' },
      ],

      trendingCard: [],

      selectedType: 0,
      activeIndex: 1,
      screenWidth: window.innerWidth,
    };
  },
  computed: {
    ...mapState(['itemSelected', 'ativeTag']),
    isSmall() {
      return this.screenWidth < 640;
    },
  },
  created() {
    window.addEventListener('resize', this.handleResize);
  },
  mounted() {
    this.getLogo();
    this.getCountry();
    this.getCategoryCardData();
  },
  unmounted() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    changeItemSelected(item) {
      this.$store.commit('setItemSelected', item);
    },
    ...mapMutations(['setActiveTag']),
    selectTag(tag) {
      this.setActiveTag(tag); // Menetapkan tag yang dipilih sebagai tag aktif

      app.config.globalProperties.$eventBus.$emit('scrollToCardSection');
    },

    getCategoryCardData() {
      this.isLoading = true;
      axios
        .get(`/categories/${this.$appId}`)
        .then((response) => {
          const data = response.data.data;

          this.categoryCard = data.map((item) => {
            return {
              id: item.category_id || '',
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
    getLogo() {
      axios
        .get(`/app/logo/${this.$appId}`)
        .then((response) => {
          const data = response.data.data;
          // console.log(data);
          this.logo = data;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
        });
    },
    getCountry() {
      axios
        .get(`/country`)
        .then((response) => {
          const data = response.data.data;
          // console.log(data);
          this.country = data.map((country) => {
            return {
              id: country.country_id,
              title: country.country_name,
              path: '#',
            };
          });
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
        });
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
    preventSubmit(event) {
      event.preventDefault();
    },
  },
};
</script>

<style scoped>
.view-all {
  position: sticky !important;
  top: 0 !important;
  left: 0 !important;
  z-index: 1000 !important;
  /* Gaya view all yang sticky */
}
.scroll-container {
  margin-top: -80px;
  overflow-x: auto;
  white-space: nowrap;
  gap: 20px;
}
.v-app-bar.v-toolbar {
  max-width: 100%;
}
.app-bar-mobile {
  height: 29vh;
}

.divider {
  background: rgb(173, 173, 173);
  width: 2px;
  height: 70px;
}

.header-info {
  align-items: center;
  gap: 25px;
  font-size: 30px;
  color: black;
  font-weight: 800;
}

.header-info-span {
  font-size: 25px;
  font-weight: 800;
}

.logo-img-container {
  height: 80px;
  min-width: 100px;
}

.logo-img {
  width: 100%;
  height: 100%;
}

.v-list-cont {
  flex-direction: column;
  gap: 20px;
}
.skeleton {
  width: 100%;
  height: 100%;
  border-radius: 0;
  z-index: 10000;
  background: linear-gradient(-90deg, #f2f2f2 0%, #e1e1e1 50%, #f2f2f2 100%);
  background-size: 400% 400%;
  animation: skeleton 1.6s ease infinite;
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
