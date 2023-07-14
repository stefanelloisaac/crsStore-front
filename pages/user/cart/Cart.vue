<!-- eslint-disable vue/attributes-order -->
<template>
  <v-container style="font-family: 'Courier New', Courier, monospace">
    <v-row style="margin-top: 2vh" class="d-flex justify-center align-center">
      <v-col
        cols="4"
        class="d-flex justify-center align-center"
        style="margin-left: -25vw"
      >
        <v-btn large color="#ff0070" style="margin-right: 4vw; color: #f6f6f6; font-weight: bold;" to="/user/home/Home"> Home </v-btn>
        <v-btn large color="#7c4dff" style="margin-right: 6vw; color: #f6f6f6; font-weight: bold"> Limpar Carrinho </v-btn>
      </v-col>
      <v-col cols="6" class="d-flex justify-center align-center">
        <h1
          class="titulo"
          style="color: #7c4dff; font-size: 50px; font-weight: lighter"
        >
          meu
        </h1>
        <h1
          class="titulo"
          style="color: #7c4dff; font-size: 50px; font-weight: bolder"
        >
          carrinho
        </h1>
      </v-col>
    </v-row>
    <v-row
      v-for="(item, i) in cart"
      :key="i"
      style="margin-top: 8vh"
      class="d-flex justify-center align-center"
    >
      <v-col cols="1" class="d-flex justify-center align-center">
        <v-btn elevation="0" x-large style="background-color: #f6f6f6">
          <v-icon style="color: grey" large @click="removeItem(item)"
            >mdi mdi-trash-can-outline</v-icon
          >
        </v-btn>
      </v-col>
      <v-col cols="4" class="d-flex justify-center align-center">
        <v-img
          class="rounded-xl"
          max-width="300"
          min-height="200"
          max-height="200"
          style="box-shadow: #7c4dff 0px 6px 9px"
          :src="item.image"
        ></v-img>
      </v-col>
      <v-col cols="5">
        <v-card
          elevation="0"
          class="mx-auto rounded-xl"
          color="#b9f6ca"
          style="width: 460px; height: 200px"
          min-height="200"
          max-height="200"
        >
          <v-chip
            class="ma-18"
            color="white"
            label
            default
            style="color: black; font-size: 16px; border: 3px solid #7c4dff"
            >{{ item.idCategory == 3 ? 'Mouse' : 'Teclado' }}</v-chip
          >
          <v-card-text
            class="d-flex justify-center align-center"
            style="color: black; font-size: 24px; font-weight: bold"
            >{{ item.name }}</v-card-text
          >
          <v-card-text
            class="d-flex justify-center align-center"
            style="color: black; font-size: 16px; font-style: italic"
            >{{ item.description }}</v-card-text
          >
          <v-card
            class="d-flex justify-center align-center rounded-xl sticky-card"
            color="#7c4dff"
            width="180"
          >
            <v-card-title style="font-weight: bold"
              >R$ {{ item.price }}</v-card-title
            >
          </v-card>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Cart',
  layout: 'LoginLayout',

  data() {
    return {
      valid: false,
      cart: [],
    }
  },

  async created() {
    await this.getUser()
  },

  methods: {
    async getUser() {
      try {
        const response = await this.$api.get('/users/by-token')
        this.cart = response.data.cart
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async removeItem(item) {
      try {
        const idItem = item.produto
        await this.$api.post(`/users/remove-item/`, { produto: idItem })
        await this.getUser()
        this.$toast.warning('Produto removido do carrinho!')
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
  },
}
</script>

<style>
.text-input-blue .v-text-field__slot input {
  color: #4a148c !important;
}

.titulo {
  text-shadow: 4px 4px #9efab8;
}

.sticky-card {
  position: absolute;
  left: 65%;
  top: 78%;
}
</style>
