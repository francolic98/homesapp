<template>
  <div class="w-full min-h-screen flex flex-col bg-gray-200">
    <div class="flex w-full h-16 items-center bg-cyan-600">
      <nuxt-link
        to="/"
        class="flex ml-12 border-b-4 border-cyan-600 hover:border-b-4 hover:border-white hover:cursor-pointer"
      >
        <span class="text-white p-2 font-semibold uppercase">Home</span>
      </nuxt-link>
    </div>
    <div class="h-ful">
      <div class="flex flex-col w-full">
        <p
          class="flex text-center font-semibold text-4xl py-4 text-gray-500 justify-center"
        >
          {{ home.title }}
        </p>
        <p class="font-bold text-xl pb-4 text-gray-500 flex justify-center">
          {{ home.address }}
        </p>
      </div>
      <div class="flex flex-col md:flex-row w-full">
        <div class="flex w-full md:w-1/2 justify-center ml-15 mt-15">
          <img
            class="flex object-fill rounded-r-lg"
            :src="`https://homehapp-api.jsteam.gaussx.com/api/media/${coverId}`"
            @error="
              $event.target.src =
                'https://www.myplace.direct/sites/default/files/property-1.jpg'
            "
          />
        </div>
        <div class="flex w-full justify-center md:w-1/2 pl-5">
          <div>
            <p
              v-if="home.price"
              class="flex font-bold text-3xl py-3 text-gray-500"
            >
              Buy for: ${{ formatPrice(home.price) }}
            </p>
            <p
              v-if="home.rentPrice"
              class="flex font-bold text-3xl py-3 text-gray-500"
            >
              Rent for: ${{ formatPrice(home.rentPrice) }}
            </p>
            <p class="flex font-semibold text-3xl text-gray-500">
              {{ home.displayAddress }}
            </p>
            <div class="flex pt-4">
              <p class="flex text-xl pr-3 text-gray-500">
                Bedrooms: {{ home.bedrooms }}
              </p>
              <p class="flex text-xl text-gray-500">
                Bathrooms: {{ home.bathrooms }}
              </p>
            </div>
            <p
              v-if="home.area"
              class="flex font-thin text-xl pt-4 text-gray-500"
            >
              Area: {{ home.area }} Sq Ft
            </p>
            <p class="flex font-thin text-xl pt-4 text-gray-500">
              {{ home.seo_desc }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      home: {},
      coverId: "",
    };
  },
  async mounted() {
    await this.getData();
  },
  methods: {
    async getData() {
      const home = await this.$axios.$get(
        `https://homehapp-api.jsteam.gaussx.com/api/home/${this.$route.params.id}`
      );
      this.home = home.data;
      this.coverId = home.data.cover.id;
    },
    formatPrice(value) {
      let val = (value / 1).toFixed(2).replace(".", ",");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
  },
};
</script>
