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
        <v-list-item v-if="data.token != null" to="/user/data/ShowUserData">
          <v-list-item-action>
            <v-icon>{{ 'mdi mdi-account-edit' }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Meus dados</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-if="data.token != null" to="/user/address/ShowUserAddress">
          <v-list-item-action>
            <v-icon>{{ 'mdi mdi-home-plus' }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Meu endereço</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-if="data.token != null" @click="logout">
          <v-list-item-action>
            <v-icon>{{ 'mdi mdi-logout-variant' }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      style="background-color: #7c4dff"
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        v-if="data.role === 'admin'"
        color="#ff0070"
        href="/admin/Category"
      >
        <v-icon style="color: white">{{
          'mdi mdi-shield-crown-outline'
        }}</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn
        v-if="data.token == null"
        color="#ff0070"
        style="margin-right: 1%; font-weight: bold"
        to="/user/login/Login"
      >
        Login
      </v-btn>
      <v-btn
        v-if="!address.id && data.token != null"
        color="#ff0070"
        style="margin-right: 1%; font-weight: bold"
        to="/user/address/PersonalAddress"
      >
        <v-icon>{{ 'mdi mdi-home-plus' }}</v-icon>
      </v-btn>
      <v-btn to="/user/cart/Cart" color="#ff0070">
        <v-icon style="color: white">
          {{ 'mdi-cart' }}
        </v-icon>
      </v-btn>
    </v-app-bar>
    <v-main style="background-color: #f6f6f6">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer style="background-color: #7c4dff" :absolute="!fixed" app>
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
      data: {},
      address: {},
      items: [
        {
          icon: 'mdi mdi-format-letter-spacing-variant',
          title: 'Home',
          to: '/user/home/Home',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },

  async created() {
    await this.getUserByToken()
    await this.getAdress()
  },

  methods: {
    logout() {
      try {
        localStorage.setItem('crsStore-api-token', '')
        location.reload()
        return this.$router.push({ name: 'user-home-Home' })
      } catch (error) {
        return this.$toast.info('Erro.')
      }
    },

    async getUserByToken() {
      try {
        const response = await this.$api.get('/users/by-token')
        this.data = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAdress() {
      try {
        const response = await this.$api.get('/address/getAll')
        this.address = response.data[0] || {}
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
  },
}
</script>
