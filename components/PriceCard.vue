<template>
  <div class="dosisFont mt-10 d-flex align-center justify-center">
    <v-scroll-y-transition>
      <v-card
        v-if="prices"
        elevation="20"
        class="mx-10 pa-8 d-flex flex-column align-center justify-center"
        color="#50df40"
      >
        <span class="dosisFont text-h4 mb-4">Com plano :D </span>
        <span class="dosisFont text-h3 mb-4"
          >R$ {{ prices.priceWithPlan }}</span
        >
      </v-card>
    </v-scroll-y-transition>
    <v-scroll-y-transition>
      <v-card
        v-if="prices"
        class="mx-10 pa-8 d-flex flex-column align-center justify-center"
        color="#ff4242"
      >
        <span class="dosisFont text-h5 mb-4">Sem plano :(</span>
        <span class="dosisFont text-h4 mb-4"
          >R$ {{ prices.priceWithoutPlan }}</span
        >
      </v-card>
    </v-scroll-y-transition>
  </div>
</template>

<script>
export default {
  name: 'PlanCard',

  props: {
    prices: {
      type: Object,
      default: null,
    },
  },

  data() {
    return {
      priceWithPlan: null,
      priceWithoutPlan: null,
    }
  },

  watch: {
    prices(newValue, oldValue) {
      console.log(newValue)
      console.log(oldValue)
    },
  },
}
</script>

<style scoped>
.dosisFont {
  font-family: 'Poppins' !important;
  text-shadow: 0 0 5px #000;
  font-weight: 700;
}
</style>
