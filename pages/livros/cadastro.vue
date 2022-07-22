<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.titulo"
              placeholder="Título"
              label="Título"
              required
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
              required
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col
            cols="2"
          >
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              item-text="nome"
              item-value="id"
              required
              :rules="rule"
            ></v-autocomplete>
          </v-col>
          <v-col
            cols="2"
          >
            <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
              required
              :rules="rule"
            ></v-autocomplete>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-container>
      <v-btn
        color="success"
        style="float: right;"
        large
        @click="cadastrar"
      >
        Cadastrar
      </v-btn>
      <v-btn
        color="error"
        large
        to="/livros"
      >
        Cancelar
      </v-btn>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',

  data () {
    return {
      valid: false,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      categorias: [],
      autores: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    this.getAutores();
    this.getCategorias();
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          idCategoria: this.livro.idCategoria,
          idAutor: this.livro.idAutor
        }
        await this.$axios.$post('http://localhost:3333/livros', livro);
        this.$toast.success('Cadastro realizado com sucesso!');
        this.$router.push('/livros');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },

    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    }
  }
}
</script>
