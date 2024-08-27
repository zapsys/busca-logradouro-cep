<template>
    <v-table id="table_1" :style="{display:t1Display}" class="mb-5 border-sm rounded-lg" fixed-header hover>
            <thead>
                <tr>
                    <th class="text-left">
                        Logradouro/Rua
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
                        Localização
                    </th>
                </tr>
            </thead>
            <tbody id="data_1">
                <tr v-for="r in ruas">
                    <td>{{ r.logradouro }} <span class="text-decoration-underline">{{ r.complemento }}</span></td>
                    <td>{{ r.bairro }}</td>
                    <td>{{ r.localidade }}</td>
                    <td>{{ r.uf }}</td>
                    <td>{{ r.cep }}</td>
                    <td><a class="text-decoration-none" :href="mapUrl+r.logradouro.replace(/ /g, '+')+'+'+r.localidade+'+'+r.uf" target="_blank">Ver mapa</a></td>
                </tr>
            </tbody>
    </v-table>
</template>
<script>
export default {
 props: { 
    ruas: {
        type: Array
    },
    t1Display: {
        type: String
    },
    mapUrl: {
        type: String
    },
    ibgeUrl: {
        type: String
    }
 }
}
</script>