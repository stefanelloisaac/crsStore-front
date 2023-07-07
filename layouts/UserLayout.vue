<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      color="#7c4dff"
      permanent
    >
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
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar style="background-color: #7c4dff" :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn outlined style="color:white" href="/admin/Category">
        <v-icon color="#f6f6f6">{{"mdi mdi-shield-crown-outline"}}</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn
        outlined
        color="#f6f6f6"
        style="margin-right: 1%;"
        to="/user/login/Login"
      >
        Login
      </v-btn>
      <v-btn
        outlined
        color="white"
      >
      <v-icon style="color:#f6f6f6" >
        {{ "mdi-cart" }}
      </v-icon>
      </v-btn> 
    </v-app-bar>
    <v-main style="background-color: #f6f6f6;">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer style="background-color: #7c4dff;" :absolute="!fixed" app>
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
      items: [
        {
          icon: 'mdi mdi-format-letter-spacing-variant',
          title: 'Home',
          to: '/user/home/Home',
        },
        {
          icon: 'mdi mdi-login',
          title: 'Login',
          to: '/user/login/Login',
        },
        {
          icon: 'mdi mdi-account-edit',
          title: 'Meus Dados',
          to: '/user/data/ShowUserData',
        },
        {
          icon: 'mdi mdi-home-plus',
          title: 'Meu Endereço',
          to: '/user/address/ShowUserAddress',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },
}
</script>