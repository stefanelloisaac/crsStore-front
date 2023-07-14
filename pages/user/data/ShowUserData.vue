<!-- eslint-disable vue/v-slot-style -->
<!-- eslint-disable vue/valid-v-slot -->
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
    <h1 class="titulo1" style="text-align: center; padding-top: 3vh; color: white; font-size: 45px;">
      meu<span class="titulo2">cadastro</span> <span class="mdi mdi-robot-outline"></span>
    </h1>
    <v-form style="padding-top: 4vh" v-model="valid">
      <v-container>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="10">
            <v-text-field
              v-model="data.username"
              class="text-input-blue"
              light
              placeholder="Username"
              color="#00897B"
              label="Username"
              filled
              rounded
              prepend-icon="mdi-account-outline"
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="10">
            <v-text-field
              v-model="data.name"
              class="text-input-blue"
              placeholder="Nome completo"
              color="#00897B"
              label="Nome completo"
              filled
              light
              prepend-icon="mdi-account-plus-outline"
              rounded
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="4">
            <v-text-field
              v-model="data.cpf"
              class="text-input-blue"
              v-mask="['###.###.###-##']"
              placeholder="CPF"
              color="#00897B"
              label="CPF"
              filled
              rounded
              prepend-icon="mdi-id-card"
              light
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="6">
            <v-text-field
              class="text-input-blue"
              v-model="data.email"
              placeholder="Email"
              color="#00897B"
              label="Email"
              filled
              rounded
              prepend-icon="mdi-email-outline"
              light
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="4">
            <v-text-field
              v-model="data.phone"
              class="text-input-blue"
              placeholder="Telefone"
              color="#00897B"
              label="Telefone"
              filled
              v-mask="['(##) ####-####', '(##) #####-####']"
              rounded
              light
              disabled
              prepend-icon="mdi-cellphone"
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col cols="6">
            <v-autocomplete
              class="text-input-blue"
              v-model="data.role"
              placeholder="Cargo"
              color="#00897B"
              label="Cargo"
              filled
              prepend-icon="mdi-account-multiple-outline"
              :items="roles"
              item-text="name"
              item-value="value"
              rounded
              light
              dense
              disabled
              background-color="#B9F6CA"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
      <v-row
      
        style="margin-left: 2vw ;padding-bottom: 2vh; margin-top: 2vh"
      >
        <v-btn
          id="editbtn"
          elevation="8"
          dark
          x-large
          style="font-weight: bold;"
          color="#ff0070"
          @click="update(data)"
        >
          Edit
        </v-btn>
      </v-row>
    <v-dialog v-model="dialog" max-width="800px" transition="dialog-top-transition">
      <v-card style="padding: 15px; font-family: 'Courier New', Courier, monospace;" color="purple darken-4">
        <v-card-title class="d-flex justify-center align-center"> Edite seus dados </v-card-title>
        <v-card-content style="padding: 15px;">
          <v-row class="d-flex justify-center align-center">
            <v-col cols="10">
              <v-text-field
                class="text-input-green"
                v-model="data.username"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Username"
                label="Username"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row class="d-flex justify-center align-center">
            <v-col cols="10">
              <v-text-field
                class="text-input-green"
                v-model="data.name"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Nome do Usuário"
                label="Nome do Usuário"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row class="d-flex justify-center align-center">
            <v-col cols="5">
              <v-text-field
                class="text-input-green"
                v-model="data.cpf"
                v-mask="'###.###.###-##'"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="CPF"
                label="CPF"
              >
              </v-text-field>
            </v-col>
            <v-col cols="5">
              <v-text-field
                class="text-input-green"
                v-model="data.email"
                outlined
                background-color="purple darken-3"
                color="#69F0AE"
                placeholder="Email do Usuário"
                label="Email do Usuário"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row class="d-flex justify-center align-center">
            <v-col cols="5">
                <v-text-field
                  class="text-input-green"
                  v-model="data.phone"
                  v-mask="['(##) ####-####', '(##) #####-####']"
                  outlined
                  background-color="purple darken-3"
                  color="#69F0AE"
                  placeholder="Telefone para contato"
                  label="Telefone para contato"
                >
                </v-text-field>
            </v-col>
            <v-col cols="5">
                <v-autocomplete
                  class="text-input-green"
                  v-model="data.role"
                  outlined
                  background-color="purple darken-3"
                  color="#69F0AE"
                  placeholder="Acesso"
                  label="Acesso"
                  :items="roles"
                  item-text="name"
                  item-value="value"
                >
                </v-autocomplete>
            </v-col>
          </v-row>
        </v-card-content>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn style="font-weight: bold;" dark color="#ff0070" large @click="persist"> Salvar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'UserShow',
  layout: 'UserLayout',

  data() {
    return {
      valid: false,
      dialog: false,
      data: {},
      roles: [
        { name: 'Cliente', value: 'customer' },
        { name: 'Entregador', value: 'deliver' },
      ],
    }
  },

  async created() {
    await this.getUserByToken()
  },

  methods: {
    update(item) {
      this.data.username = item.username
      this.data.cpf = item.cpf
      this.data.name = item.name
      this.data.phone = item.phone
      this.data.role = item.role
      this.data.email = item.email
      this.dialog = true
    },

    async getUserByToken() {
      try {
        const response = await this.$api.get('/users/by-token')
        this.data = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async persist() {
      try {
        const request = {
          username: this.data.username,
          cpf: this.data.cpf,
          name: this.data.name,
          phone: this.data.phone,
          role: this.data.role,
          email: this.data.email,
        }
        
        await this.$api.patch(`/users/${this.id}`, request);
        this.$toast.success('Dado editado com êxito.');
        
        this.username = null
        this.cpf = null
        this.name = null
        this.phone = null
        this.role = null
        this.email = null
        this.dialog = false
        await this.getUserByToken()
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

.text-input-green .v-text-field__slot input {
  color: #f6f6f6 !important;
  font-weight: bold;
}

.editbtn:disabled {
  cursor: not-allowed;
  background-color: red;
  opacity: 0.1;
}

.titulo1 {
  text-shadow: -2px 1px #311B92;
  font-weight: 100;
}

.titulo2 {
  text-shadow: -2px 1px #311B92;
  font-weight: 900;
}
</style>
