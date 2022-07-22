<template>
  <v-container>
    <h1>Cadastro de Usuários</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="usuario.id"
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
              v-model="usuario.nome"
              placeholder="Nome"
              label="Nome"
              outlined
              required
              :rules="rule"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              placeholder="E-mail"
              label="E-mail"
              outlined
              required
              :rules="rule"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="CPF/CNPJ"
              label="CPF/CNPJ"
              outlined
              required
              :rules="rule"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              label="Telefone"
              outlined
              required
              :rules="rule"
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
        to="/usuarios"
      >
        Cancelar
      </v-btn>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroUsuariosPage',

  data () {
    return {
      valid: false,
      usuario: {
        id: null,
        nome: null,
        email: null,
        telefone: null,
        cpfcnpj: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let usuario = {
          nome: this.usuario.nome,
          email: this.usuario.email,
          telefone: this.usuario.telefone,
          cpfcnpj: this.usuario.cpfcnpj 
        };
        await this.$axios.$post('http://localhost:3333/usuarios', usuario);
        this.$toast.success('Cadastro realizado com sucesso!');
        this.$router.push('/usuarios');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    }
  }
}
</script>
