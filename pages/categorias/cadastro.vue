<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="categoria.id"
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
              v-model="categoria.nome"
              placeholder="Nome"
              label="Nome"
              required
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-container>
      <v-btn
        color="success"
        style="float: right;"
        large
        @click="persistir"
      >
        Salvar
      </v-btn>
      <v-btn
        color="error"
        large
        to="/categorias"
      >
        Cancelar
      </v-btn>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroCategoriasPage',

  data () {
    return {
      valid: false,
      categoria: {
        id: null,
        nome: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async persistir () {
      try {
        //primeiro valida-se se o formulário está preenchido
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }

        //montamos a variárel categoria para enviar nos posts
        let categoria = {
          nome: this.categoria.nome,
        };

        //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
        if (!this.categoria.id) {
          await this.$axios.$post('http://localhost:3333/categorias', categoria);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/categorias');
        }

        await this.$axios.$post(`http://localhost:3333/categorias/${this.categoria.id}`, categoria);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/categorias');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
    }
  }
}
</script>
