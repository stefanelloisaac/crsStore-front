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
        <h1 style="color: black">Cadastro de Pedidos</h1>
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
            <v-col>
              <v-autocomplete
                v-model="idAdress"
                :items="address"
                item-text="street"
                item-value="id"
                label="Endereço"
              />
            </v-col>
            <v-col>
              <v-autocomplete
                v-model="idPayment"
                :items="payments"
                item-text="name"
                item-value="id"
                label="Pagamento"
              />
            </v-col>
            <v-col>
              <v-autocomplete
                v-model="idCupom"
                :items="cupoms"
                item-text="code"
                item-value="id"
                label="Usuário"
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <v-text-field
                v-model="status"
                outlined
                background-color="blue-grey darken-3"
                color="#FFECB3"
                placeholder="Status"
                label="Status"
              >
              </v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                v-model="total"
                outlined
                background-color="blue-grey darken-3"
                color="#FFECB3"
                placeholder="Total"
                label="Total"
              >
              </v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                v-model="totalDiscount"
                outlined
                background-color="blue-grey darken-3"
                color="#FFECB3"
                placeholder="Desconto total"
                label="Desconto total"
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
      users: [],
      payments: [],
      cupoms: [],
      address: [],
      id: null,
      status: null,
      total: null,
      totalDiscount: null,
      dialog: false,
      headers: [
        {
          text: 'ID',
          value: 'id',
          align: 'center',
        },
        {
          text: 'id Endereço',
          value: 'idAdress',
          align: 'center',
        },
        {
          text: 'id Cupom',
          value: 'idCupom',
          align: 'center',
        },
        {
          text: 'id Pagamento',
          value: 'idPayment',
          align: 'center',
        },
        {
          text: 'Status',
          value: 'status',
          align: 'center',
        },
        {
          text: 'Desconto',
          value: 'totalDiscount',
          align: 'center',
        },
        {
          text: 'Total',
          value: 'total',
          align: 'center',
        },
        { text: 'Ações', value: 'actions', style: 'center', filterable: false },
      ],
    }
  },

  computed: {
    tituloDialog() {
      return this.id ? 'Editar Registro' : 'Criar Registro'
    },
  },

  async created() {
    await this.getAllOrders()
    await this.getAllUsers()
    await this.getAllAddress()
    await this.getAllCupoms()
    await this.getAllPayments()
  },

  methods: {
    update(item) {
      this.id = item.id
      this.status = item.status
      this.total = item.total
      this.totalDiscount = item.totalDiscount
      this.idAdress = item.idAdress
      this.idPayment = item.idPayment
      this.idCupom = item.idCupom
      this.dialog = true
    },

    async persist() {
      try {
        const request = {
          status: this.status,
          total: this.total,
          totalDiscount: this.totalDiscount,
          idAdress: this.idAdress,
          idPayment: this.idPayment,
          idCupom: this.idCupom,
        }
        if (this.id) {
          await this.$api.patch(`/orders/${this.id}`, request)
          this.$toast.success('Dado editado com êxito.')
        } else {
          await this.$api.post(`/orders/`, request)
          this.$toast.success('Dado adicionado com êxito.')
        }
        this.status = null
        this.total = null
        this.totalDiscount = null
        this.idAdress = null
        this.idPayment = null
        this.idCupom = null
        this.id = null
        this.dialog = false
        await this.getAllOrders()
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async destroy(item) {
      try {
        await this.$api.delete(`/orders/deletar/${item.id}`)
        await this.getAllOrders()
        this.$toast.success('Dado excluído com êxito.')
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAllOrders() {
      try {
        const response = await this.$api.get('/orders')
        this.items = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAllUsers() {
      try {
        const response = await this.$api.get('/users/')
        this.users = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAllAddress() {
      try {
        const response = await this.$api.get('/address/')
        this.address = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAllPayments() {
      try {
        const response = await this.$api.get('/payments/')
        this.payments = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },

    async getAllCupoms() {
      try {
        const response = await this.$api.get('/cupoms/')
        this.cupoms = response.data
      } catch (error) {
        return this.$toast.warning('Ocorreu um erro.')
      }
    },
  },
}
</script>

<style></style>
