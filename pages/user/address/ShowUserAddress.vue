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
    <h1
      class="titulo1"
      style="
        text-align: center;
        padding-top: 3vh;
        color: white;
        font-size: 45px;
      "
    >
      meu<span class="titulo2">endereço</span>
      <span class="mdi mdi-emoticon-poop-outline"></span>
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
    <v-row style="margin-left: 2vw; padding-bottom: 2vh; margin-top: 2vh">
      <v-btn
        id="editbtn"
        elevation="8"
        dark
        x-large
        style="font-weight: bold"
        color="#ff0070"
        @click="update(data)"
      >
        Edit
      </v-btn>
    </v-row>
    <v-dialog
      v-model="dialog"
      max-width="800px"
      transition="dialog-top-transition"
    >
      <v-card
        style="padding: 15px; font-family: 'Courier New', Courier, monospace"
        color="purple darken-4"
      >
        <v-card-title class="d-flex justify-center align-center">
          Edite seu endereço
        </v-card-title>
        <v-card-content style="padding: 15px">
          <v-row class="d-flex justify-center align-center">
            <v-col cols="2">
              <v-text-field
                class="text-input-green"
                v-model="zipCode"
                v-mask="['#####-###']"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="CEP"
                label="CEP"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row class="d-flex justify-center align-center">
            <v-col cols="8">
              <v-text-field
                class="text-input-green"
                v-model="data.street"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Logradouro"
                label="Logradouro"
              >
              </v-text-field>
            </v-col>
            <v-col cols="2">
              <v-text-field
                class="text-input-green"
                v-model="data.numberForget"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Número"
                label="Número"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row class="d-flex justify-center align-center">
            <v-col cols="4">
              <v-text-field
                class="text-input-green"
                v-model="data.district"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Bairro"
                label="Bairro"
              >
              </v-text-field>
            </v-col>
            <v-col cols="5">
              <v-text-field
                class="text-input-green"
                v-model="data.city"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Cidade"
                label="Cidade"
              >
              </v-text-field>
            </v-col>
            <v-col cols="1">
              <v-text-field
                class="text-input-green"
                v-model="data.state"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="UF"
                label="UF"
              >
              </v-text-field>
            </v-col>
          </v-row>
        </v-card-content>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            style="font-weight: bold"
            dark
            color="#ff0070"
            large
            @click="persist"
          >
            Salvar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'Adress',
  layout: 'UserLayout',

  data() {
    return {
      valid: null,
      zipCode: null,
      data: {
        id: null,
        city: null,
        state: null,
        idUser: null,
        district: null,
        numberForget: null,
        street: null,
      },
      response: null,
      baseURL: `https://viacep.com.br/ws/`,
      rule: { must: (v) => !!v || 'Esse campo é obrigatorio' },
      dialog: false,
    }
  },

  watch: {
    async zipCode(novoCep) {
      if (novoCep.length === 9) {
        await this.getCep()
      } else {
        this.response = null
      }
    },
  },

  async created() {
    await this.getAdress()
  },

  methods: {
    update(item) {
      this.zipCode = item.zipCode
      this.data.city = item.city
      this.data.state = item.state
      this.data.district = item.district
      this.data.street = item.street
      this.data.numberForget = item.numberForget
      this.dialog = true
    },

    async getCep() {
      const url = `${this.baseURL}${this.zipCode}/json/`
      await this.$axios.get(url).then((resp) => {
        const data = resp.data
        if (!data.erro) {
          this.data.city = data.localidade
          this.data.district = data.bairro
          this.data.state = data.uf
          this.data.street = data.logradouro
        } else {
          this.$toast.error('CEP não encontrado!')
        }
      })
    },

    async getAdress() {
      try {
        const response = await this.$api.get('/address/getAll')
        this.data = response.data[0]
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async persist() {
      try {
        const request = {
          zipCode: this.zipCode,
          city: this.data.city,
          state: this.data.state,
          street: this.data.street,
          numberForget: this.data.numberForget,
          district: this.data.district,
        }
        await this.$api.patch(`/address/${this.data.id}`, request)
        this.$toast.success('Dado editado com êxito.')

        this.zipCode = null
        this.city = null
        this.state = null
        this.street = null
        this.numberForget = null
        this.district = null
        this.dialog = false
        await this.getAdress()
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

.titulo1 {
  text-shadow: -2px 1px #311b92;
  font-weight: 100;
}

.titulo2 {
  text-shadow: -2px 1px #311b92;
  font-weight: 900;
}
</style>
