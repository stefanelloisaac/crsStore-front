<template>
  <v-container style="font-family: 'Courier New', Courier, monospace">
    <v-row class="d-flex justify-center align-center" style="margin-top: 2%">
      <h1 style="font-size: 64px; color: #7c4dff">
        <span class="mdi mdi-ideogram-cjk-variant"></span> akira's custom shop
      </h1>
    </v-row>
    <v-container>
      <v-row style="margin-top: 6%">
        <template v-for="(category, i) in categories">
          
          <v-col :key="i" class="mt-2" cols="12">
            <strong style="font-size: 30px; color: #7c4dff; margin-left: 2%">{{
              category.name
            }}</strong>
          </v-col>
          <v-slide-group
            :key="i"
            show-arrows
            style="margin-top: -2%"
            class="pa-4"
            center-active
          >
            <v-slide-item
              v-for="(product, j) in category.products"
              :key="`${i}${j}`"
              v-slot="{ active, toggle }"
              style="background-color: #f6f6f6"
              fixed
            >
              <v-hover v-slot="{ hover }">
              <v-card
                v-model="model"
                :hover="active ? true : false"
                :color="active ? 'deep-purple accent-1' : 'green accent-1'"
                class="ma-4"
                height="240"
                width="320"
                @click="toggle"
              >
                <v-img height="75%" :src="product.image">
                  <v-expand-transition>
                    <div
                      v-if="hover"
                      class="d-flex transition-fast-in-fast-out purple darken-2 v-card--reveal text-h5 white--text"
                      style="height: 100%;"
                    >
                      Preço: R$ {{ product.price }} <br>
                      Infos: {{ product.description }}
                    </div>
                  </v-expand-transition>
                </v-img>
                <v-card-title
                  class="d-flex justify-center align-center"
                  style="font-weight: bold"
                  >{{ product.name }}</v-card-title
                >
                <v-row class="fill-height" align="center" justify="center">
                  <v-scale-transition>
                    <v-icon v-if="active" color="white" size="48"></v-icon>
                  </v-scale-transition>
                </v-row>
              </v-card>
            </v-hover>
            </v-slide-item>
          </v-slide-group>
        
        </template>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'Home',
  layout: 'UserLayout',
  data: () => ({
    model: null,
    categories: [],
  }),

  async created() {
    await this.getAllProducts()
  },
  methods: {
    async getAllProducts() {
      try {
        const response = await this.$api.get('/categories/')
        this.categories = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
  },
}
</script>

<style>
.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: 0.8;
  position: absolute;
  width: 100%;
}
</style>
