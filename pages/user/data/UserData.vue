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
      Edite seus dados <span class="mdi mdi-robot-outline"></span>
    </h1>
    <v-form style="padding-top: 10px" v-model="valid">
      <v-container>
        <v-row>
          <v-col cols="3">
            <v-text-field
              v-model="users.id"
              class="text-input-blue"
              placeholder="Código"
              label="Código"
              color="#00897B"
              filled
              light
              rounded
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
            <v-text-field
              v-model="users.username"
              class="text-input-blue"
              light
              placeholder="Username"
              color="#00897B"
              label="Username"
              filled
              rounded
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
            <v-text-field
              v-model="users.cpf"
              class="text-input-blue"
              v-mask="['###.###.###-##']"
              placeholder="CPF"
              color="#00897B"
              label="CPF"
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
              v-model="users.name"
              class="text-input-blue"
              placeholder="Nome"
              color="#00897B"
              label="Nome"
              filled
              light
              rounded
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
            <v-text-field
              class="text-input-blue"
              v-model="users.email"
              placeholder="Email"
              color="#00897B"
              label="Email"
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
              v-model="users.phone"
              class="text-input-blue"
              placeholder="Telefone"
              color="#00897B"
              label="Telefone"
              filled
              v-mask="['(##) ####-####', '(##) #####-####']"
              rounded
              light
              dense
              background-color="#B9F6CA"
            />
          </v-col>
          <v-col>
            <v-text-field
              class="text-input-blue"
              v-model="users.role"
              placeholder="Cargo"
              color="#00897B"
              label="Cargo"
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
              v-model="users.passwordHash"
              placeholder="PasswordHash"
              color="#00897B"
              label="PasswordHash"
              filled
              rounded
              light
              dense
              background-color="#B9F6CA"
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
        to="/public/user/users"
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
  layout: 'user',

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
      },
      rule: [(v) => !!v || 'Esse campo é obrigatório'],
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
          passwordHash: this.users.passwordHash,
          role: this.users.role,
          email: this.users.email,
        }

        if (!this.users.id) {
          await this.$api.$post('/users', users)
          this.$router.push('/public/user/users')
          return this.$toast.success(`Endereço cadastrado com sucesso!`)
        }

        await this.$api.$post(`/users/${this.users.id}`, users)
        this.$router.push('/public/user/users')
        this.$toast.success('Cadastro atualizado com sucesso!')
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!')
      }
    },
    async getById(id) {
      const users = await this.$api.$get(`/users/${id}`)
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
