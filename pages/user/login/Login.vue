<!-- eslint-disable vue/attributes-order -->
<template>
  <v-form>
    <v-container>
      <v-row>
        <v-col cols="5">
          <h1 style="font-size: 50px; margin-top: 45%; margin-left: 20%">
            <span
              style="
                font-family: 'Courier New', Courier, monospace;
                font-size: 21px;
                color: #00897b;
              "
            >
              <p>good for health,</p>
              <p style="margin-top: -20px">
                bad for education <span class="mdi mdi-pill-off"></span>
              </p>
            </span>

            <strong
              style="
                font-family: 'Courier New', Courier, monospace;
                color: #7c4dff;
              "
            >
              akira's
              <span class="mdi mdi-copyleft"></span>
            </strong>
          </h1>
        </v-col>
        <v-col>
          <v-container
            class="elevation-10 rounded-xl"
            style="
              border: 2px solid #00897b;
              background-color: #b9f6ca;
              border-radius: 1%;
              width: 70%;
              margin-top: 13%;
              margin-left: 7%;
              font-family: 'Courier New', Courier, monospace;
            "
          >
            <v-container style="padding: 8%; background-color: #b9f6ca">
              <v-row>
                <v-col>
                  <v-text-field
                    v-model="login.username"
                    class="text-input-blue"
                    label="Username"
                    placeholder="Username"
                    filled
                    rounded
                    dense
                    required
                    light
                    color="#00897B"
                    prepend-inner-icon="mdi-email-outline"
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col>
                  <v-text-field
                    class="text-input-blue"
                    label="Senha"
                    v-model="login.password"
                    style="margin-top: -8%"
                    filled
                    rounded
                    dense
                    :append-icon="show ? 'mdi-eye-off' : 'mdi-eye'"
                    @click:append="toggleShow"
                    :type="show ? 'text' : 'password'"
                    light
                    placeholder="Senha"
                    color="#00897B"
                    prepend-inner-icon="mdi-lock-outline"
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col style="margin-top: -10%">
                  <a
                    href="/user/login/Forget"
                    class=""
                    style="margin-top: -5%; color: red; margin-left: 12vw; font-size: 14px;"
                    >Esqueci minha senha</a
                  >
                </v-col>
              </v-row>
              <v-row>
                <v-col>
                  <v-btn x-large block color="#7c4dff" style="font-weight: bold;" @click="efetuarLogin"
                    >Entrar</v-btn
                  >
                </v-col>
              </v-row>
              <v-row style="margin-top: 10%">
                <v-col>
                  <p
                    style="
                      margin-top: -5%;
                      color: black;
                      text-align: center;
                      font-size: 90%;
                      margin: auto;
                    "
                  >
                    Não tem uma conta?
                    <strong>
                      <a style="color: #6200EA; font-weight: bolder;" href="/user/data/PersonalData"
                        >Registre-se</a
                      >
                    </strong>
                  </p>
                </v-col>
              </v-row>
              <v-row class="d-flex justify-center align-center" style="margin-top: 10%;">
                <v-spacer></v-spacer>
                <v-btn outlined color="green accent-1">
                  <v-icon color="blue darken-4">
                    mdi-facebook
                  </v-icon>
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn outlined color="green accent-1">
                  <v-icon color="pink accent-4">
                    mdi-instagram
                  </v-icon>
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn outlined color="green accent-1">
                  <v-icon color="blue">
                    mdi-twitter
                  </v-icon>
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn href="https://github.com/stefanelloisaac" outlined color="green accent-1">
                  <v-icon color="black">
                    mdi-github
                  </v-icon>
                </v-btn>
                <v-spacer></v-spacer>
              </v-row>
            </v-container>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'Users',
  layout: 'UserLayout',

  data() {
    return {
      valid: false,
      show:false,
      login:{
        username: null,
        password: null
      },
      rule:{
        must: v => !!v || 'Esse campo é obrigatorio',}
    }
  },

  methods: {
    async efetuarLogin(){
      const log ={
        username: this.login.username,
        password: this.login.password
      }
      try {
        const response = await this.$api.post('/users/login', log)
        if(response.type === "success"){
          localStorage.setItem("crsStore-api-token", response.data.token)
          this.$toast.success("Você está logado!")
          this.$router.push("/")
        }else{
          this.$toast.error(response.data.message)
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o login!');
      }
    },
    toggleShow(){
      this.show = !this.show
    },
  }
}
</script>

<style>
.text-input-blue .v-text-field__slot input {
  color: #4a148c !important;
}
</style>
