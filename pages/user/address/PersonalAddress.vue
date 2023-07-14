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
      Cadastre seu endereço <span class="mdi mdi-emoticon-poop-outline"></span>
    </h1>
    <v-form style="padding-top: 4vh" v-model="valid">
      <v-container>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="3">
            <v-text-field
              v-model="zipCode"
              class="text-input-blue"
              light
              placeholder="CEP"
              color="#00897B"
              label="CEP"
              v-mask="['#####-###']"
              filled
              rounded
              prepend-icon="mdi-city-variant-outline"
              dense
              background-color="#B9F6CA"
              :rules="[rule.must]"
            />
          </v-col>
          <v-col cols="5">
            <v-text-field
              v-model="adress.city"
              class="text-input-blue"
              placeholder="Cidade"
              color="#00897B"
              label="Cidade"
              filled
              rounded
              light
              dense
              background-color="#B9F6CA"
              :rules="[rule.must]"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model="adress.state"
              class="text-input-blue"
              placeholder="Estado"
              color="#00897B"
              label="Estado"
              filled
              light
              rounded
              dense
              background-color="#B9F6CA"
              :rules="[rule.must]"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="5">
            <v-text-field
              class="text-input-blue"
              v-model="adress.street"
              placeholder="Logradouro"
              color="#00897B"
              label="Logradouro"
              filled
              rounded
              prepend-icon="mdi-map-marker-outline"
              light
              dense
              background-color="#B9F6CA"
              :rules="[rule.must]"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model="adress.numberForget"
              class="text-input-blue"
              placeholder="Número"
              color="#00897B"
              label="Número"
              filled
              rounded
              light
              dense
              background-color="#B9F6CA"
              :rules="[rule.must]"
            />
          </v-col>
          <v-col cols="3">
            <v-text-field
              class="text-input-blue"
              v-model="adress.district"
              placeholder="Bairro"
              color="#00897B"
              label="Bairro"
              background-color="#B9F6CA"
              filled
              rounded
              light
              dense
              :rules="[rule.must]"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-row
      class="d-flex justify-center align-center"
      style="padding-bottom: 15px; margin-top: 2vh"
    >
      <v-btn
        light
        x-large
        style="margin-right: 2%"
        @click="persistir"
        color="#FFECB3"
      >
        Registrar
      </v-btn>
      <v-btn
        light
        x-large
        style="margin-left: 2%"
        to="../../user/home/Home"
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
      valid: false,
      zipCode: null,
      adress: {
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

  created() {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async getCep() {
      const url = `${this.baseURL}${this.zipCode}/json/`
      await this.$axios.get(url).then((resp) => {
        const data = resp.data
        if (!data.erro) {
          this.adress.city = data.localidade
          this.adress.district = data.bairro
          this.adress.state = data.uf
          this.adress.street = data.logradouro
        } else {
          this.$toast.error('CEP não encontrado!')
        }
      })
    },

    async persistir() {
      try {
        const adress = {
          city: this.adress.city,
          state: this.adress.state,
          zipCode: this.zipCode,
          district: this.adress.district,
          numberForget: this.adress.numberForget,
          street: this.adress.street,
        }

        if (!this.adress.id) {
          await this.$api.$post('/address', adress)
          this.$router.push('/user/home/Home')
          location.reload()
          return this.$toast.success(`Endereço cadastrado com sucesso!`)
        }

        await this.$api.$post(`/address/${this.adress.id}`, adress)
        this.$router.push('/user/home/Home')
        this.$toast.success('Cadastro atualizado com sucesso!')
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!')
      }
    },
    async getById(id) {
      const adress = await this.$api.$get(`/address/${id}`)
      this.adress = adress.data
      this.zipCode = adress.zipCode
    },
  },
}
</script>

<style>
.text-input-blue .v-text-field__slot input {
  color: #4a148c !important;
}
</style>
