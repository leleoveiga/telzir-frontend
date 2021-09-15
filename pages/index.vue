<template>
  <v-row class="content" justify="center" align="center">
    <v-col cols="12">
      <v-card class="transition pt-12 pb-12">
        <PlanForm @onCalculatePrice="onCalculatePrice" />
        <PriceCard :prices="prices" />
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'Home',

  data() {
    return {
      prices: null,
    }
  },

  methods: {
    onCalculatePrice(data) {
      this.prices = data
    },
  },
}
</script>

<style scoped>
@media (min-width: 960px) {
  .content {
    margin: 16px;
  }
}
.transition {
  -webkit-transition: height 0.25s ease;
  -moz-transition: height 0.25s ease;
  transition: height 0.25s ease;
}
</style>
