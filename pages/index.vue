<template>
  <v-row class="content" justify="center" align="center">
    <v-col cols="12">
      <v-card class="transition pt-12 pb-12">
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'Home',

  data() {
    return {
    }
  },

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
