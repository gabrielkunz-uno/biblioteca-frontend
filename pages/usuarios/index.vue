<template>
  <v-container>
    <h1>Consulta de Usuários</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            large
            color="primary"
            @click="getUsuarios"
          >
            Pesquisar
            <v-icon style="margin-left: 5%">
              mdi-magnify
            </v-icon>
          </v-btn>
          <v-btn
            large
            color="success"
            to="/usuarios/cadastro"
          >
            Cadastrar
            <v-icon style="margin-left: 5%">
              mdi-plus-circle-outline
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editItem(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaUsuariosPage',

  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },
        {
          text: 'E-mail',
          align: 'center',
          sortable: false,
          value: 'email',
        },
        { text: "", value: "actions" }
      ],
      usuarios: []
    }
  },

  created () { //executado toda vez que a pagina é carregada
    this.getUsuarios()
  },

  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
    },

    async deletar (usuario) {
      try {
        if (confirm(`Deseja deletar o registro id ${usuario.id} - ${usuario.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuario.id });
          this.$toast.success(response.message)
          this.getUsuarios();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao deletar o registro');
      }
    }
  }

}
</script>