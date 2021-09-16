<template>
  <v-form v-if="origins" ref="form" v-model="validForm">
    <v-row class="mx-12 d-flex align-center justify-center">
      <v-col cols="12" lg="2" sm="3">
        <v-select
          v-model="originSelected"
          hint="Origem da chamada"
          :items="origins"
          :rules="[(value) => !!value || 'Selecione uma origem!']"
          persistent-hint
          return-object
          single-line
          required
          outlined
          dense
        ></v-select>
      </v-col>
      <v-col cols="12" lg="2" sm="3">
        <v-select
          v-model="destinySelected"
          hint="Destino da chamada"
          :items="destinies"
          :rules="[(value) => !!value || 'Selecione um destino!']"
          persistent-hint
          return-object
          single-line
          required
          outlined
          dense
        ></v-select>
      </v-col>
      <v-col cols="12" lg="2" sm="3">
        <v-text-field
          v-model="time"
          hint="Duração da chamada"
          :rules="[
            (value) =>
              (Number.isInteger(Number(value)) && value > 0) ||
              'Digite a duração da chamada!',
          ]"
          persistent-hint
          outlined
          dense
        ></v-text-field>
      </v-col>
    </v-row>

    <div class="mt-14 d-flex flex-column">
      <span class="text-body-1 text-center">
        Selecione um plano pra calcular os preços!
      </span>
      <v-item-group class="d-flex flex-wrap align-center justify-center">
        <div v-for="plan in plans" :key="plan.code">
          <v-item v-slot="{ active, toggle }">
            <v-card
              :color="active ? 'primary' : '' + 'accent'"
              class="mx-4 mt-6 d-flex align-center flex-column justify-center"
              style="position: relative"
              :elevation="active ? 18 : 0"
              height="200"
              min-width="300"
              @click="toggleAndSelect(plan, toggle, active)"
            >
              <span class="text-h3 dosisFont">{{ plan.name }}</span>
              <span class="mt-6 text-h6">{{ plan.description }}</span>
              <v-scroll-y-transition>
                <div
                  v-if="active"
                  class="selected justify-self-end flex-grow-1 text-center"
                  style=""
                >
                  <v-icon large>mdi-check-circle-outline</v-icon>
                  Selecionado
                </div>
              </v-scroll-y-transition>
            </v-card>
          </v-item>
        </div>
      </v-item-group>
      <div class="mt-10 d-flex align-center justify-center">
        <v-scroll-y-transition>
          <v-card
            v-if="planRule"
            class="pa-4 d-flex align-center justify-center"
            color="warning"
          >
            <v-icon large>mdi-exclamation-thick</v-icon>
            Selecione algum plano !
          </v-card>
        </v-scroll-y-transition>
      </div>
      <v-btn
        x-large
        class="dosisFont text-h5 primary mt-10 align-self-center"
        :loading="loadingCalc"
        @click="calculatePrice"
      >
        CALCULAR CUSTOS!
      </v-btn>
    </div>
  </v-form>
</template>

<script>
// import axios from 'axios'
export default {
  name: 'PlanForm',

  data() {
    return {
      origins: [],
      destinies: [],
      plans: [],

      time: 0,
      originSelected: '',
      destinySelected: '',
      planSelected: '',

      loadingCalc: false,
      validForm: false,
      planRule: false,
    }
  },
  computed: {},
  watch: {
    originSelected() {
      this.getValidDestinies()
    },
  },
  mounted() {
    this.$axios
      .$get('ddds/list')
      .then((res) => {
        this.originSelected = res[0]
        this.origins = res
      })
      .catch((error) => {
        this.$emit('onRequestError', error)
      })
    this.$axios
      .$get('plans/list')
      .then((res) => {
        this.plans = res
      })
      .catch((error) => {
        this.$emit('onRequestError', error)
      })
  },

  methods: {
    validatePlan() {
      if (!this.planSelected) {
        this.planRule = true
        setTimeout(() => (this.planRule = false), 3000)

        return false
      } else {
        return true
      }
    },
    resetValidation() {
      this.$refs.form.resetValidation()
    },
    toggleAndSelect(plan, toggle, active) {
      toggle()
      if (!active) this.planSelected = plan.code
      else this.planSelected = ''
    },
    calculatePrice() {
      if (this.$refs.form.validate() && this.validatePlan()) {
        const origin = this.originSelected
        const destiny = this.destinySelected
        const time = this.time
        const planCode = this.planSelected
        this.loadingCalc = true
        this.$axios
          .$get('plans/prices', {
            params: {
              origin,
              destiny,
              time,
              planCode,
            },
          })
          .then((res) => {
            this.$emit('onCalculatePrice', res)
            this.loadingCalc = false
          })
          .catch((error) => {
            this.$emit('onRequestError', error)
            this.loadingCalc = false
          })
      }
    },
    getValidDestinies() {
      this.$axios
        .$get(`ddds/destinies/${this.originSelected}`)
        .then((res) => {
          this.destinies = res
        })
        .catch((error) => {
          this.$emit('onRequestError', error)
        })
    },
  },
}
</script>

<style scoped>
.selected {
  position: absolute;
  top: 80%;
}
.dosisFont {
  font-family: 'Dosis' !important;
  font-weight: 700;
}
</style>
