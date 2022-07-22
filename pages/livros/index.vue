<template>
  <v-container>
    <h1>Consulta de Livros</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            large
            color="primary"
            @click="getLivros"
          >
            Pesquisar
            <v-icon style="margin-left: 5%">
              mdi-magnify
            </v-icon>
          </v-btn>
          <v-btn
            large
            color="success"
            to="/livros/cadastro"
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
        :items="livros"
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
  name: 'ConsultaLivrosPage',

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
          text: 'Título',
          align: 'center',
          sortable: false,
          value: 'titulo',
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: false,
          value: 'categoria.nome',
        },
        {
          text: 'Autor',
          align: 'center',
          sortable: false,
          value: 'autor.nome',
        },
        { text: "", value: "actions" }
      ],
      livros: []
    }
  },

  created () { //executado toda vez que a pagina é carregada
    this.getLivros()
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },

    async deletar (livro) {
      try {
        if (confirm(`Deseja deletar o registro id ${livro.id} - ${livro.titulo}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livro.id });
          this.$toast.success(response.message)
          this.getLivros();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao deletar o registro');
      }
    }
  }

}
</script>