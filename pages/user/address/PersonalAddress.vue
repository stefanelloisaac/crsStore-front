<!-- eslint-disable vue/attributes-order -->
<template>
  <v-container
    style="
      border: 1px #55d6be;
      background-color: #55d6be;
      margin-top: 50px;
      padding-bottom: 35px;
      font-family: 'Courier New', Courier, monospace;
    "
    class="elevation-20 rounded-xl"
  >
    <h1 style="text-align: center; padding-top: 20px">
      Cadastre seu endereço <span class="mdi mdi-emoticon-poop-outline"></span>
    </h1>
    <v-form style="padding-top: 10px" v-model="valid">
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="adress.idUser"
              class="text-input-blue"
              placeholder="Código"
              label="Código"
              filled
              light
              rounded
              dense
              color="#00897B"
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
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
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
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
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
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
            />
          </v-col>
          <v-col>
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
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              class="text-input-blue"
              v-model="adress.street"
              placeholder="Logradouro"
              color="#00897B"
              label="Logradouro"
              filled
              rounded
              light
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
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
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-row
      class="d-flex justify-center align-center"
      style="padding-bottom: 10px; padding-top: 10px"
    >
      <v-btn
        light
        style="margin-right: 20px"
        @click="persistir"
        color="#FFECB3"
      >
        Cadastrar
      </v-btn>
      <v-btn
        light
        style="margin-left: 20px"
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
      rule: [(v) => !!v || 'Esse campo é obrigatório'],
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
          idUser: this.adress.idUser,
        }

        if (!this.adress.id) {
          await this.$api.$post('/address', adress)
          this.$router.push('/public/user/adress')
          return this.$toast.success(`Endereço cadastrado com sucesso!`)
        }

        await this.$api.$post(`/address/${this.adress.id}`, adress)
        this.$router.push('/public/user/adress')
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
