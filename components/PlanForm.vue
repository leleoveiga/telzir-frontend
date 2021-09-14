<template>
  <v-form v-if="origins" ref="form" v-model="validForm">
    <v-row class="mx-12 d-flex align-center justify-center">
      <v-col cols="12" sm="3">
        <v-select
          v-model="originSelected"
          hint="Origem da chamada"
          :items="origins"
          :rules="[(value) => !!value || 'Selecione uma origem!']"
          persistent-hint
          return-object
          single-line
          required
        ></v-select>
      </v-col>
      <v-col cols="12" sm="3">
        <v-select
          v-model="destinySelected"
          hint="Destino da chamada"
          :items="destinies"
          :rules="[(value) => !!value || 'Selecione um destino!']"
          persistent-hint
          return-object
          single-line
          required
        ></v-select>
      </v-col>
      <v-col cols="12" sm="3">
        <v-text-field
          v-model="time"
          hint="Duração da chamada"
          :rules="[
            (value) =>
              (Number.isInteger(Number(value)) && value > 0) ||
              'Digite a duração da chamada!',
          ]"
          persistent-hint
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
              :elevation="active ? 24 : 0"
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
        large
        class="pa-8 dosisFont text-h4 primary mt-10 align-self-center"
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

      validForm: false,
      planRule: false,
    }
  },
  computed: {},
  watch: {
    originSelected(newValue, oldValue) {
      this.getValidDestinies()
    },
  },
  mounted() {
    this.$axios.$get('ddds/list').then((res) => {
      this.originSelected = res[0]
      this.origins = res
    })
    this.$axios.$get('plans/list').then((res) => {
      this.plans = res
    })
  },

  methods: {
    validatePlan() {
      console.log('validate plan')
      console.log(this.originSelected)
      console.log(this.destinySelected)
      console.log(this.planSelected)
      //   this.$refs.form.validate()
      if (!this.planSelected) {
        console.log(this.planRule, 'plan rule')
        this.planRule = true
        setTimeout(() => (this.planRule = false), 3000)

        return false
      } else {
        return true
      }
    },
    reset() {
      this.$refs.form.reset()
    },
    resetValidation() {
      this.$refs.form.resetValidation()
    },
    toggleAndSelect(plan, toggle, active) {
      toggle()
      console.log(active, 'ativo?')
      console.log(plan)
      if (!active) this.planSelected = plan.planCode
      else this.planSelected = ''
    },
    calculatePrice() {
      if (this.$refs.form.validate() && this.validatePlan()) {
        console.log('achoq  ta valido')
        const origin = this.originSelected
        const destiny = this.destinySelected
        const time = this.time
        const planCode = this.planSelected
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
            console.log('🚀 ~ file: PlanForm.vue ~ line 212~ .then ~ res', res)
            this.$emit('onCalculatePrice', res)
          })
      } else {
        console.log('acho q n ta valido')
      }
    },
    getValidDestinies() {
      this.$axios.$get(`ddds/destinies/${this.originSelected}`).then((res) => {
        console.log('🚀 ~ file: PlanForm.vue ~ line 193~ .then ~ res', res)
        this.destinies = res
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
  text-shadow: 0 0 4px #000;
  font-weight: 700;
}
</style>
