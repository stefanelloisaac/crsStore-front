<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      color="#253C59"
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
    <v-app-bar style="background-color: #253C59" :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn icon href="http://localhost:3000/user/home/Home">
        <v-icon>mdi mdi-shield-crown-outline</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main style="background-color: #e0dede;">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer style="background-color: #253C59;" :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-book',
          title: 'Categorias',
          to: '/admin/Category',
        },
        {
          icon: 'mdi mdi-cash-fast',
          title: 'Pagamentos',
          to: '/admin/Payment',
        },
        {
          icon: 'mdi mdi-account-circle',
          title: 'Usuários',
          to: '/admin/User',
        },
        {
          icon: 'mdi mdi-ticket-percent',
          title: 'Cupons',
          to: '/admin/Cupom',
        },
        {
          icon: 'mdi mdi-basket',
          title: 'Produtos',
          to: '/admin/Product',
        },
        {
          icon: 'mdi mdi-map-marker',
          title: 'Endereços',
          to: '/admin/Address',
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