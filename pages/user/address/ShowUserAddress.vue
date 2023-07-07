<!-- eslint-disable vue/attributes-order -->
<template>
  <v-container
    style="
      width: 1000px;
      border: 1px #55d6be;
      background-color: #55d6be;
      margin-top: 3vh;
      padding-bottom: 3vh;
      font-family: 'Courier New', Courier, monospace;
    "
    class="elevation-20 rounded-xl"
  >
    <h1 style="text-align: center; padding-top: 3vh">
      Meu endereço <span class="mdi mdi-emoticon-poop-outline"></span>
    </h1>
    <v-form style="padding-top: 4vh" v-model="valid">
      <v-container>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="3">
            <v-text-field
              v-model="data.zipCode"
              class="text-input-blue"
              light
              placeholder="CEP"
              color="#00897B"
              label="CEP"
              v-mask="['#####-###']"
              filled
              rounded
              disabled
              prepend-icon="mdi-city-variant-outline"
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="5">
            <v-text-field
              v-model="data.city"
              class="text-input-blue"
              placeholder="Cidade"
              color="#00897B"
              label="Cidade"
              filled
              rounded
              light
              disabled
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model="data.state"
              class="text-input-blue"
              placeholder="Estado"
              color="#00897B"
              label="Estado"
              filled
              light
              rounded
              disabled
              dense
              background-color="#B9F6CA"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="5">
            <v-text-field
              class="text-input-blue"
              v-model="data.street"
              placeholder="Logradouro"
              color="#00897B"
              label="Logradouro"
              filled
              rounded
              prepend-icon="mdi-map-marker-outline"
              light
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model="data.numberForget"
              class="text-input-blue"
              placeholder="Número"
              color="#00897B"
              label="Número"
              filled
              rounded
              disabled
              light
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="3">
            <v-text-field
              class="text-input-blue"
              v-model="data.district"
              placeholder="Bairro"
              color="#00897B"
              label="Bairro"
              background-color="#B9F6CA"
              filled
              rounded
              disabled
              light
              dense
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-row
      class="d-flex justify-center align-center"
      style="padding-bottom: 15px; margin-top: 2vh"
    >
      <v-btn light x-large style="margin-right: 2%" color="#FFECB3">
        Editar
      </v-btn>
      <v-btn
        light
        x-large
        style="margin-left: 2%"
        to="/public/user/adress"
        color="#EF9A9A"
      >
        Cancelar
      </v-btn>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Adress',
  layout: 'UserLayout',

  data() {
    return {
      valid: null,
      data: [],
    }
  },

  async created() {
    await this.getAdress()
  },

  methods: {
    async getAdress() {
      try {
        const response = await this.$api.get('/address/getAll')
        this.data = response.data[0]
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
</style>
