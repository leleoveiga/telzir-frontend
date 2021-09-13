<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" color="#201db2" class="" fixed app>
      <div class="my-10 d-flex align-center justify-center">
        <NuxtLink to="/">
          <v-img
            :src="require('/assets/telzir-icon.png')"
            max-height="75"
            max-width="75"
          ></v-img>
        </NuxtLink>
      </div>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :src="require('/assets/celulares.png')"
      color="#fcb69f"
      fixed
      prominent
      fade-img-on-scroll
      shrink-on-scroll
      app
    >
      <template #img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(55,236,186,.7), rgba(25,32,72,.7)"
        ></v-img>
      </template>
      <v-app-bar-nav-icon class="appBarNavIcon" @click="drawer = !drawer" />
      <v-toolbar-title class="my-auto text-h4"> Telzir </v-toolbar-title>
      <v-btn
        v-if="miniVariant"
        id="menu-button"
        icon
        dark
        @click="miniVariant = !miniVariant"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
      <v-spacer />
      <v-btn
        rel="noreferrer"
        class="mt-1"
        href="https://github.com/leleoveiga/telzir-frontend"
        target="_blank"
        text
      >
        <v-icon class="mr-1">mdi-github</v-icon>
        <span class="mr-2"> Github </span>
        <v-icon small>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-calculator',
          title: 'Calculadora',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Sobre',
          to: '/about',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      navBarTitle: 'Telzir',
    }
  },
}
</script>
<style scoped>
@media (min-width: 1264px) {
  .appBarNavIcon {
    display: none;
  }
}
</style>
