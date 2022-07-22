<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="autor.id"
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
              v-model="autor.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.email"
              placeholder="E-mail"
              label="E-mail"
              :rules="rule"
              required
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
        @click="cadastrar"
      >
        Cadastrar
      </v-btn>
      <v-btn
        color="error"
        large
        to="/autores"
      >
        Cancelar
      </v-btn>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutoresPage',

  data () {
    return {
      valid: false,
      autor: {
        id: null,
        nome: null,
        email: null,
        senha: null,
        confirmacao: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ],
    }
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email
        };
        await this.$axios.$post('http://localhost:3333/autores', autor);
        this.$toast.success('Cadastro realizado com sucesso!');
        this.$router.push('/autores');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    }
  }
}
</script>
