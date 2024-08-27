<template>
  <v-app :theme="theme">
    <v-app-bar class="px-3">
      <v-spacer></v-spacer>
      <v-btn :prepend-icon="theme === 'light' ? 'mdi-weather-sunny' : 'mdi-weather-night'" text="Tema" slim
        @click="onClick"></v-btn>
    </v-app-bar>
    <v-main>
      <v-container class="my-3">
        <h1 class="fw-bold text-center my-3 text-light">Buscar informações de uma rua/logradouro</h1>
        <Form1 />
        <h1 class="fw-bold text-center my-3 text-light">Buscar informações de logradouro por CEP</h1>
        <Form2 />
        <Data2 />
      </v-container>
    </v-main>
    <v-footer>
      <v-row justify="center" no-gutters>
        <v-col class="text-center" cols="12">
          <p>
            &copy;{{ new Date().getFullYear() }} <a href="https://zapwebsites.com.br" class="text-decoration-none"
              rel="norefferer" target="_blank">Zapwebsites</a>
            <v-btn href="https://github.com/zapsys/busca-logradouro-cep" icon="mdi-github" class="mx-4 float-end" variant="text" role="link" target="_blank"></v-btn>
          </p>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script setup>
import { ref } from 'vue'

const theme = ref('light')

function onClick() {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}
</script>
<script>
import Form1 from './components/Form1.vue'
import Form2 from './components/Form2.vue'

export default {
  name: 'BuscaLogradouro',
  components: {
    Form1, Form2
  },
  data: () => ({
    model: true,
  }),
}
</script>
