<template>
    <v-table id="table_2" :style="{ display: t2Display }" class="mb-3 border-sm rounded-lg" fixed-header hover>
      <thead>
        <tr>
          <th class="text-left">
            Logradouro/Rua
          </th>
          <th class="text-left">
            Complemento
          </th>
          <th class="text-left">
            Bairro/Distrito
          </th>
          <th class="text-left">
            Localidade
          </th>
          <th class="text-left">
            UF
          </th>
          <th class="text-left">
            CEP
          </th>
          <th class="text-left">
            IBGE
          </th>
        </tr>
      </thead>
      <tbody id="data_2">
        <tr>
          <td v-for="c, i in info" :key="i">
          <td v-if="i == 6"><a class="text-decoration-none" :href="ibgeUrl" target="_blank">{{ c }}</a></td>
          <td v-else>{{ c }}</td>
          </td>
        </tr>
      </tbody>
    </v-table>
  </template>
  <script>
  export default {
    props: {
      info: {
        type: Array
      },
      t2Display: {
        type: String
      },
      ibgeUrl: {
        type: String
      }
    },
  }
  </script>