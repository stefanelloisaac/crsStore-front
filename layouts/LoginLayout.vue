<template>
  <v-app>
    <v-app-bar
      style="background-color: #7c4dff; height: 5vh"
      :absolute="!fixed"
      app
    >
      <v-toolbar-title
        style="
          font-family: 'Courier New', Courier, monospace;
          margin-bottom: 1%;
        "
      >
        akira's custom shop
      </v-toolbar-title>
      <v-spacer> </v-spacer>
      <v-toolbar-title
        style="
          font-family: 'Courier New', Courier, monospace;
          margin-bottom: 1%;
        "
      >
        <span class="mdi mdi-keyboard-outline"></span> keyboards
        <span class="mdi mdi-pokeball"></span> mouses
        <span class="mdi mdi-ghost-outline"></span>
      </v-toolbar-title>
    </v-app-bar>
    <v-main style="background-color: #f6f6f6">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      style="background-color: #7c4dff; height: 5vh"
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'UserLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },
}
</script>
