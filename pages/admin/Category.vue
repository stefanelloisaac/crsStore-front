<!-- eslint-disable vue/valid-v-slot -->
<!-- eslint-disable vue/v-slot-style -->
<template>
  <v-container
    style="
      font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
        Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue',
        sans-serif;
    "
  >
    <v-row>
      <v-col class="d-flex justify-center align-center">
        <h1 style="color: black">Cadastro de Categorias</h1>
      </v-col>
    </v-row>
    <v-row class="d-flex justify-center align-center">
      <v-btn fab small color="#BF8D30" @click="dialog = true">
        <v-icon> mdi-plus </v-icon>
      </v-btn>
    </v-row>
    <v-row style="padding-top: 20px" class="d-flex justify-center align-center">
      <v-card
        class="elevation-10"
        width="900"
        style="background-color: #465d6b; color: black"
      >
        <v-card-title style="color: black">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          style="background-color: #465d6b"
          :headers="headers"
          :items="items"
          :search="search"
        >
          <template v-slot:item.actions="{ item }">
            <v-icon medium color="#FFECB3" @click="update(item)">
              mdi mdi-pencil-circle
            </v-icon>
            <v-icon medium color="#EF9A9A" @click="destroy(item)">
              mdi mdi-delete-circle
            </v-icon>
          </template>
        </v-data-table>
      </v-card>
    </v-row>
    <v-dialog v-model="dialog">
      <v-card style="padding: 15px" color="blue-grey darken-2">
        <v-card-title>
          {{ tituloDialog }}
        </v-card-title>
        <v-card-content style="padding: 15px">
          <v-row>
            <v-col cols="2">
              <v-text-field
                v-model="id"
                outlined
                disabled
                background-color="blue-grey darken-2"
                color="#FFECB3"
                placeholder="ID da Categoria"
                label="ID da Categoria"
              >
              </v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                v-model="name"
                outlined
                color="#FFECB3"
                background-color="blue-grey darken-3"
                placeholder="Nome da Categoria"
                label="Nome da Categoria"
              >
              </v-text-field>
            </v-col>
          </v-row>
        </v-card-content>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="#BF8D30" @click="persist"> Salvar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'Index',
  data() {
    return {
      search: null,
      items: [],
      dialog: false,
      name: null,
      id: null,
      headers: [
        {
          text: 'ID',
          value: 'id',
          align: 'center',
        },
        {
          text: 'Categoria',
          value: 'name',
          align: 'center',
        },
        { text: 'Ações', value: 'actions', align: 'center', filterable: false },
      ],
    }
  },

  computed: {
    tituloDialog() {
      return this.id ? 'Editar Registro' : 'Criar Registro'
    },
  },

  async created() {
    await this.getAllCategories()
  },
  methods: {
    update(item) {
      this.name = item.name
      this.id = item.id
      this.dialog = true
    },
    async persist() {
      try {
        const request = {
          name: this.name,
        }
        if (this.id) {
          await this.$api.patch(`/categories/${this.id}`, request)
          this.$toast.success('Dado editado com êxito.')
        } else {
          await this.$api.post(`/categories/`, request)
          this.$toast.success('Dado adicionado com êxito.')
        }
        this.name = null
        this.id = null
        this.dialog = false
        await this.getAllCategories()
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
    async destroy(item) {
      try {
        await this.$api.delete(`/categories/destroy/${item.id}`)
        await this.getAllCategories()
        this.$toast.success('Dado excluído com êxito.')
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
    async getAllCategories() {
      try {
        const response = await this.$api.get('/categories/')
        this.items = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
  },
}
</script>

<style></style>
