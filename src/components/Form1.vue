<template>
    <v-form ref="form" class="border-sm rounded-lg">
        <v-container>
            <v-row>
                <v-col>
                    <v-select v-model="selectedUF" variant="outlined" id="uf" name="uf" label="Estado" placeholder="Estado"
                        :items="estados" @update:model-value="getCidades()"></v-select>
                </v-col>
                <v-col>
                    <v-select v-model="selectedCidade" variant="outlined" id="cidades" name="cidades" label="Cidade"
                        placeholder="Cidade" :items="cidades"></v-select>
                </v-col>
            </v-row>
            <v-row>
                <v-col class="mb-2">
                    <v-text-field v-model="selectedRua" variant="outlined" label="Rua/Logradouro" id="rua" name="rua"
                        required></v-text-field>
                </v-col>
            </v-row>
            <v-btn class="rounded-pill mr-3" color="primary" @click="pesquisaRua()">Pesquisar</v-btn>
            <v-btn class="rounded-pill" color="grey" @click="limpaTabela()">Limpar</v-btn>
        </v-container>
    </v-form>
    <Data1 :mapUrl="mapUrl" :t1Display="t1Display" :ruas="ruasInfo" />
</template>
<script>
import Data1 from '@/components/Data1.vue'

export default {
    components: {
        Data1
    },
    data: () => ({
        estados: [],
        cidades: [],
        selectedUF: null,
        selectedRua: null,
        selectedCidade: null,
        selectedCep: null,
        t1Display: 'none',
        ruasInfo: [],
        estadoNome: null,
        mapUrl: 'https://www.google.com/maps/place/',
    }),
    created() {
        this.getEstados()
    },
    methods: {
        getEstados() {
            fetch('https://servicodados.ibge.gov.br/api/v1/localidades/estados?orderBy=nome')
                .then((response) => {
                    response.json().then(data => {
                        data.forEach((sigla, id) => {
                            sigla = data[id].sigla
                            this.estados.push(sigla)
                        })
                    })
                })
                .catch((err) => {
                    console.error(err);
                })
        },
        getCidades() {
            this.cidades = [],
                fetch(`https://servicodados.ibge.gov.br/api/v1/localidades/estados/${this.selectedUF}/municipios?orderBy=nome`)
                    .then((response) => {
                        response.json().then(data => {
                            data.forEach((nome, id) => {
                                nome = data[id].nome
                                this.cidades.push(nome)
                                this.selectedCidade = this.cidades.slice(0, 1)
                            })
                        })
                    })
                    .catch((err) => {
                        console.error(err)
                    })
        },
        pesquisaRua() {
            if (this.selectedUF == null) {
                alert('Selecione um estado antes!')
                uf.focus()
                return false
            }
            if (this.selectedRua == null) {
                alert('Informe o nome do logradouro/rua para pesquisar!')
                rua.focus()
                return false
            }
            //Verifica se campo rua possui valor informado.
            if (this.selectedRua && this.selectedRua != null) {
                this.selectedRua.replace(/ /g, '+')
                fetch(`https://viacep.com.br/ws/${this.selectedUF}/${this.selectedCidade}/${this.selectedRua}/json/`)
                    .then((response) => {
                        response.json().then(data => {
                            this.ruasInfo = data
                            //console.log(this.ruasInfo)
                            if (data.length == 0) {
                                alert('Nenhuma informação encontrada, tente novamente!')
                                rua.focus()
                            }
                            else {
                                this.t1Display = 'block'
                            }
                        })
                    })
            }
        },
        limpaTabela() {
            //data_1.innerHTML = ''
            this.t1Display = 'none'
            this.selectedRua = null
            this.ruasInfo = []
            this.mapUrl = 'https://www.google.com/maps/place/'
            rua.focus()
            console.clear()
        },
    },
}
</script>