<template>
  <div class="w-full">
    <TheNavbar />
    <div>
      <div class="flex flex-wrap w-full py-5 px-4 bg-gray-300">
        <div
          class="px-5 py-3 shadow-lg rounded-md text-center w-1/2 min-w-min hover:cursor-pointer hover:shadow-2xl"
          @click="toggleTabs(1)"
          :class="{
            'text-cyan-600 bg-white': filters.listingTypes !== 1,
            'text-white bg-cyan-600': filters.listingTypes === 1,
          }"
        >
          <a class="font-bold text-lg uppercase hover:cursor-pointer">Buy</a>
        </div>
        <div
          class="px-5 py-3 shadow-lg rounded-md text-center w-1/2 min-w-min hover:cursor-pointer hover:shadow-2xl"
          @click="toggleTabs(2)"
          :class="{
            'text-cyan-600 bg-white': filters.listingTypes !== 2,
            'text-white bg-cyan-600': filters.listingTypes === 2,
          }"
        >
          <a class="font-bold text-lg uppercase">Rent</a>
        </div>
      </div>
      <div class="w-full flex flex-wrap justify-center pt-5 bg-cyan-800">
        <div class="min-w-fit pr-2">
          <input
            class="text-xl px-3 py-2 rounded-md text-gray-500"
            type="text"
            name="input"
            @input="debounceInput"
            v-model="filters.searchText"
            placeholder="Search..."
          />
        </div>
        <button
          class="w-1/12 min-w-min text-cyan-600 bg-white text-xl border-2 border-cyan-600 rounded-md px-3 py-1 hover:bg-gray-200" @click="toggle"
        >
        <span class="flex flex-nowrap justify-center items-center w-auto">
          <fa icon="fa-solid fa-filter" class="pr-2"></fa>Filter
        </span>
        </button>
      </div>
      <div class="bg-cyan-800 p-5">
        <div v-show="showFilters">
          <form @submit.prevent="onSubmit" class="flex flex-col items-center">
            <div class="min-w-fit bg-white w-1/4 rounded-md p-4">
              <div class="pb-3">
                <span class="text-lg font-semibold">Bedrooms</span>
                <div class="mt-2">
                  <input
                    type="radio"
                    id="one"
                    :value="1"
                    v-model="filters.bedrooms"
                  />
                  <label for="one">1</label>
                  <input
                    type="radio"
                    id="two"
                    :value="2"
                    v-model="filters.bedrooms"
                  />
                  <label for="two">2</label>
                  <input
                    type="radio"
                    id="three"
                    :value="3"
                    v-model="filters.bedrooms"
                  />
                  <label for="three">3</label>
                  <input
                    type="radio"
                    id="four"
                    :value="4"
                    v-model="filters.bedrooms"
                  />
                  <label for="four">4</label>
                  <input
                    type="radio"
                    id="five"
                    :value="5"
                    v-model="filters.bedrooms"
                  />
                  <label for="five">5+</label>
                </div>
              </div>
              <div class="pb-3">
                <span class="text-lg font-semibold">Bathrooms</span>
                <div class="mt-2">
                  <input
                    type="radio"
                    id="one"
                    :value="1"
                    v-model="filters.bathrooms"
                  />
                  <label for="one">1</label>
                  <input
                    type="radio"
                    id="two"
                    :value="2"
                    v-model="filters.bathrooms"
                  />
                  <label for="two">2</label>
                  <input
                    type="radio"
                    id="three"
                    :value="3"
                    v-model="filters.bathrooms"
                  />
                  <label for="three">3</label>
                  <input
                    type="radio"
                    id="four"
                    :value="4"
                    v-model="filters.bathrooms"
                  />
                  <label for="four">4</label>
                  <input
                    type="radio"
                    id="five"
                    :value="5"
                    v-model="filters.bathrooms"
                  />
                  <label for="five">5+</label>
                </div>
              </div>
              <div class="mb-4">
                <span class="text-lg font-semibold">Min Price</span>
                <input
                  class="border-2 pl-2 min-w-fit"
                  type="Number"
                  min="0"
                  placeholder="enter price"
                  v-model="filters.price_from"
                />
              </div>
              <div class="mb-4">
                <span class="text-lg font-semibold">Max Price</span>
                <input
                  class="border-2 pl-2"
                  type="Number"
                  min="0"
                  placeholder="enter price"
                  v-model="filters.price_to"
                />
              </div>
              <button
                class="w-full flex justify-center bg-white py-1 rounded-lg border-2 border-cyan-700 text-cyan-700 font-semibold hover:cursor-pointer hover:shadow-xl hover:bg-gray-200"
              >
                Find
              </button>
            </div>
          </form>
        </div>

        <div
          class="flex flex-wrap justify-center gap-7 py-5 mx-auto"
          :class="{ hidden: this.filters.listingTypes != 1 }"
        >
          <div
            v-for="home in listHomes"
            v-if="home.price"
            :key="home.id"
            class="px-5"
          >
            <PropertyCell :home="home" class="h-full" />
          </div>
        </div>

        <div
          class="flex flex-wrap justify-center gap-7 py-5"
          :class="{ hidden: this.filters.listingTypes != 2 }"
        >
          <div
            v-for="home in listHomes"
            v-if="home.rentPrice"
            :key="home.id"
            class="px-5"
          >
            <PropertyCell :home="home" class="h-full" />
          </div>
        </div>

        <div class="flex mt-5 bg-cyan-700 px-5 py-4 w-full mx-auto">
          <div class="flex w-full justify-between min-w-fit">
            <div class=" border-2 border-white hover:scale-110">
              <button
                class=" text-white text-lg font-bold uppercase px-3"
                @click="prevPage"
              >
                Prev
              </button>
            </div>
            <button class="text-white text-xl font-bold uppercase px-1">
              {{ pagination && pagination.page }}
            </button>
            <div class=" border-2 border-white hover:scale-110">
              <button
                class="w-fit text-white text-lg font-bold uppercase px-3"
                @click="nextPage"
              >
                Next
              </button>
            </div>

          </div>
        </div>
      </div>
    </div>
    <TheFooter />
  </div>
</template>

<script>
import _ from "lodash";

import TheNavbar from "../components/TheNavbar.vue";
import TheFooter from "../components/TheFooter.vue";
export default {
  components: {
    TheNavbar,
    TheFooter,
    // PropertyCell
  },
  data() {
    return {
      listHomes: [],
      pagination: {},
      activeTab: 1,
      showFilters: false,
      filters: {
        listingTypes: 1,
        limit: 30,
        page: 1,
        bedrooms: null,
        bathrooms: null,
        price_from: null,
        price_to: null,
        searchText: null,
      },
    };
  },
  methods: {
    readQueryParams() {
      const {
        listingTypes,
        limit,
        page,
        bedrooms,
        bathrooms,
        price_from,
        price_to,
        searchText,
      } = this.$route.query;
      this.filters = {
        listingTypes: listingTypes || 1,
        limit: limit || 30,
        page: parseInt(page) || 1,
        bedrooms: bedrooms,
        bathrooms: bathrooms,
        price_from: price_from,
        price_to: price_to,
        searchText: searchText,
      };
    },
    debounceInput: _.debounce(async function (e) {
      this.filters.page = 1;
      await this.getData();
    }, 500),

    async onSubmit() {
      this.listHomes = [];
      this.filters.page = 1;
      await this.getData();
    },
    toggle() {
      this.showFilters = !this.showFilters
    },
    async getData() {
      const url = "https://homehapp-api.jsteam.gaussx.com/api/home";
      const response = await this.$axios.$get(`${url}`, {
        params: { ...this.filters },
      });
      this.$router.push({ query: { ...this.filters } });
      this.pagination = response.data.pagination;
      this.listHomes.splice(0, this.listHomes.length, ...response.data.data);
    },
    async toggleTabs(tabNumber) {
      this.listHomes = []
      this.filters.listingTypes = tabNumber;
      this.filters.page = 1;
      await this.getData();
    },
    async nextPage() {
      if (this.filters.page != this.pagination.lastPage) {
        this.filters.page += parseInt(1);
        await this.getData(this.filters.page);
        window.scrollTo(0, 0);
      }
    },
    async prevPage() {
      if (this.filters.page != 1) {
        this.filters.page -= parseInt(1);
        await this.getData(this.filters.page);
        window.scrollTo(0, 0);
      }
    },
  },
  async mounted() {
    this.readQueryParams();
    await this.getData();
  },
};
</script>
