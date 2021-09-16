<template>
  <v-row class="poppinsFont mt-14 d-flex align-center justify-center">
    <v-scroll-y-transition>
      <v-card
        v-if="prices"
        elevation="20"
        class="
          mx-10
          pa-8
          d-flex
          flex-column
          align-center
          justify-center
          comPlano
        "
        color="#50df40"
      >
        <span class="poppinsFont text-h4 mb-4">Com plano :D </span>
        <span class="poppinsFont text-h3 mb-4"
          >R$ {{ prices.priceWithPlan }}</span
        >
      </v-card>
    </v-scroll-y-transition>
    <v-scroll-y-transition>
      <v-card
        v-if="prices"
        class="
          mx-10
          pa-8
          d-flex
          flex-column
          align-center
          justify-center
          semPlano
        "
        color="#ff4242"
      >
        <span class="poppinsFont text-h5 mb-4">Sem plano :(</span>
        <span class="poppinsFont text-h4 mb-4"
          >R$ {{ prices.priceWithoutPlan }}</span
        >
      </v-card>
    </v-scroll-y-transition>
  </v-row>
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
}
</script>

<style scoped>
.poppinsFont {
  font-family: 'Poppins' !important;
  font-weight: 700;
}
.comPlano {
  text-shadow: 0px 5px 4px rgba(0, 0, 0, 0.4), 0px 8px 13px rgba(0, 0, 0, 0.1),
    0px 18px 23px rgba(0, 0, 0, 0.1);
}
@media screen and (max-width: 678px) {
  .semPlano {
    margin-top: 32px;
  }
}
</style>
