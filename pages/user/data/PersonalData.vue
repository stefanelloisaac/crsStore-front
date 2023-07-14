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
      Cadastre seus dados <span class="mdi mdi-robot-outline"></span>
    </h1>
    <v-form style="padding-top: 4vh" v-model="valid">
      <v-container>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="10">
            <v-text-field
              v-model="users.username"
              class="text-input-blue"
              light
              placeholder="Username"
              color="#00897B"
              label="Username"
              filled
              rounded
              prepend-icon="mdi-account-outline"
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="5">
            <v-text-field
              v-model="users.password"
              class="text-input-blue"
              light
              placeholder="Digite sua senha"
              color="#00897B"
              label="Digite sua senha"
              filled
              prepend-icon="mdi-lock-outline"
              rounded
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
          <v-col cols="5">
            <v-text-field
              v-model="users.password2"
              class="text-input-blue"
              placeholder="Confirme sua senha"
              color="#00897B"
              label="Confirme sua senha"
              filled
              prepend-icon="mdi-lock-check-outline"
              light
              rounded
              dense
              background-color="#B9F6CA"
              :rules="[rule.password, rule.equalPassword]"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="10">
            <v-text-field
              v-model="users.name"
              class="text-input-blue"
              placeholder="Nome completo"
              color="#00897B"
              label="Nome completo"
              filled
              light
              prepend-icon="mdi-account-plus-outline"
              rounded
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="4">
            <v-text-field
              v-model="users.cpf"
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
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
          <v-col cols="6">
            <v-text-field
              class="text-input-blue"
              v-model="users.email"
              placeholder="Email"
              color="#00897B"
              label="Email"
              filled
              rounded
              prepend-icon="mdi-email-outline"
              light
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
        </v-row>
        <v-row class="d-flex justify-center alignt-center">
          <v-col cols="4">
            <v-text-field
              v-model="users.phone"
              class="text-input-blue"
              placeholder="Telefone"
              color="#00897B"
              label="Telefone"
              filled
              v-mask="['(##) ####-####', '(##) #####-####']"
              rounded
              light
              prepend-icon="mdi-cellphone"
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
            />
          </v-col>
          <v-col cols="6">
            <v-autocomplete
              class="text-input-blue"
              v-model="users.role"
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
              clearable
              dense
              background-color="#B9F6CA"
              :rules="[rule.password]"
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
        REGISTRAR
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
  name: 'Users',
  layout: 'UserLayout',

  data() {
    return {
      valid: false,
      users: {
        id: null,
        username: null,
        cpf: null,
        passwordHash: null,
        role: null,
        name: null,
        phone: null,
        email: null,
        password: null,
        password2: null,
      },
      rule: {
        password: (v) => !!v || 'Esse campo é obrigatorio',
        equalPassword: (v) => v === this.users.password || 'Senha diferente!',
      },
      roles: [
        { name: 'Cliente', value: 'customer' },
        { name: 'Entregador', value: 'deliver' },
      ],
    }
  },

  created() {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir() {
      try {
        const users = {
          username: this.users.username,
          cpf: this.users.cpf,
          name: this.users.name,
          phone: this.users.phone,
          password: this.users.password,
          role: this.users.role,
          email: this.users.email,
        }

        if (!this.users.id) {
          await this.$api.post('/users', users)
          this.$router.push('/user/login/Login')
          return this.$toast.success(`Dados cadastrados com sucesso!`)
        }

        await this.$api.patch(`/users/${this.users.id}`, users).token
        // localStorage.setItem('crsStore-api-token', token)
        this.$router.push('/user/login/Login')
        this.$toast.success('Cadastro atualizado com sucesso!')
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!')
      }
    },
    async getById(id) {
      const users = await this.$api.get(`/users/${id}`)
      this.users = users.data
    },
  },
}
</script>

<style>
.text-input-blue .v-text-field__slot input {
  color: #4a148c !important;
}
</style>
