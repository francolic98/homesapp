<template>
  <div
    class="flex flex-col bg-gray-900 max-w-sm rounded-lg mt-5 text-center text-gray-300 y-100 hover:scale-105 hover:cursor-pointer"
    @click="$router.push(`/home/${home.id}`)"
  >
    <img
      class="flex object-fill h-48 w-96 rounded-t-lg"
      :src="`https://homehapp-api.jsteam.gaussx.com/api/media/${home.cover.id}`"
      @error="
        $event.target.src =
          'https://www.myplace.direct/sites/default/files/property-1.jpg'
      "
    />
    <div class="flex flex-1 flex-col h-full p-5 justify-center">
      <div v-if="home.price">
        <p class="text-2xl font-bold">${{ formatPrice(home.price) }}</p>
      </div>
      <div v-if="home.rentPrice">
        <p class="text-2xl font-bold">${{ formatPrice(home.rentPrice) }}/mo</p>
      </div>

      <p class="text-xl font-semibold">{{ home.title }}</p>
      <div class="flex justify-center">
        <p class="pr-2 font-semibold text-lg">{{ home.bedrooms }} Bedrooms</p>
        <p class="pl-2 font-semibold text-lg">{{ home.bathrooms }} Bathrooms</p>
      </div>
      <p class="font-bold">{{ home.address }}</p>
      <p class="pb-4">{{ home.seo_desc }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "PropertyCell",
  props: {
    home: {},
  },
  methods: {
    formatPrice(value) {
      let val = (value / 1).toFixed(2).replace(".", ",");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
  },
};
</script>
