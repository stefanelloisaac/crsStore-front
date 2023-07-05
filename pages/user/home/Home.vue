<template>
  <v-container style="font-family: 'Courier New', Courier, monospace">
    <v-row class="d-flex justify-center align-center" style="margin-top: 2%">
      <h1
        class="titulo"
        style="
          font-size: 64px;
          color: #7c4dff;
          font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande',
            'Lucida Sans', Arial, sans-serif;
        "
      >
        <span class="mdi mdi-ideogram-cjk-variant"></span> akira's custom shop
        <span class="mdi mdi-ideogram-cjk"></span>
      </h1>
    </v-row>
    <v-container>
      <v-row style="margin-top: 6%">
        <template v-for="(category, i) in categories">
          <v-col :key="i" class="mt-2" cols="12">
            <strong style="font-size: 30px; color: #7c4dff; margin-left: 6%">{{
              category.name
            }}</strong>
          </v-col>
          <v-slide-group
            :key="i"
            show-arrows
            prev-icon="mdi-chevron-triple-left"
            next-icon="mdi-chevron-triple-right"
            light
            style="margin-top: -6%;"
            class="pa-6"
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
                  :color="active ? 'deep-purple accent-3' : 'grey darken-3'"
                  class="ma-14"
                  height="320"
                  width="400"
                  @click="toggle"
                >
                  <v-img height="75%" :src="product.image">
                    <v-expand-transition>
                      <div
                        v-if="hover"
                        class="d-flex transition-fast-in-fast-out green accent-4 v-card--reveal text-h4 white--text"
                        style="height: 100%"
                      >
                        Preço: R$ {{ product.price }}
                      </div>
                    </v-expand-transition>
                  </v-img>
                  <v-card-title
                    class="d-flex justify-center align-center"
                    style="font-weight: bold; color: #f6f6f6;"
                    >{{ product.name }}</v-card-title
                  >
                  <v-card-subtitle class="d-flex justify-center align-center" style="font-style: italic; color: #f6f6f6;">
                    {{ product.description }}
                  </v-card-subtitle>
                  <v-card-actions class="d-flex justify-center align-center">
                    <v-btn light color="green accent-3" variant="text">
                      Add to cart <span class="mdi mdi-cart-plus"></span>
                    </v-btn>
                  </v-card-actions>
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

.titulo {
  text-shadow: 4px 4px #9efab8;
}
</style>
