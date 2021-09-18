<template>
  <v-row class="content" justify="center" align="center">
    <v-col cols="12">
      <v-card class="transition pt-12 pb-12">
        <v-snackbar v-model="snackbar" app color="error" timeout="3000" top>
          <b>{{ snackbarMessage }}</b>
        </v-snackbar>
        <PlanForm
          @onCalculatePrice="onCalculatePrice"
          @onRequestError="onRequestError"
        />
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

      snackbarMessage: '',
      snackbar: false,
    }
  },

  methods: {
    onCalculatePrice(data) {
      this.prices = data
    },
    onRequestError(snackbarData) {
      if (snackbarData.response) {
        const errorCode = snackbarData.response.data.status
        const message = snackbarData.response.data.error
        this.snackbarMessage = `Erro ${errorCode}: ${message}`
        this.snackbar = true
      } else {
        const message = 'Serviço indisponível!'
        const errorCode = 502
        this.snackbarMessage = `Erro ${errorCode}: ${message}`
        this.snackbar = true
      }
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
