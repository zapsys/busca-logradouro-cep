<template>
    <v-form ref="cepForm" class="border-sm rounded-lg bg-light">
        <v-container>
            <v-row>
                <v-col class="mb-2">
                    <v-text-field v-model="selectedCep" variant="outlined" id="cep" name="cep" label="CEP" type="text"
                        :rules="[rules.onlyNum]" maxlength="9" required></v-text-field>
                </v-col>
            </v-row>
            <v-btn class="rounded-pill mr-3" color="primary" @click="pesquisaCep()">Pesquisar</v-btn>
            <v-btn class="rounded-pill" color="grey" @click="limpaCep()">Limpar</v-btn>
        </v-container>
    </v-form>
    <Data2 :ibgeUrl="ibgeUrl" :t2Display="t2Display" :info="cepInfo" />
</template>
<script>
import Data2 from '@/components/Data2.vue'
const numbers = /^[\d-]+$/
export default {
    components: {
        Data2
    },
    data: () => ({
        selectedCep: null,
        cepInfo: [],
        t2Display: 'none',
        rules: {
            onlyNum: value => numbers.test(value) || 'Usar CEP no formato 99999-999 ou apenas 8 dígitos!',
        }
    }),
    methods: {
        pesquisaCep() {
            let validaCep = /^[0-9]{8,9}$/
            if (this.selectedCep == null) {
                alert('Informe um CEP para pesquisar!')
                cep.focus()
                return false
            }
            //Verifica se campo cep possui valor informado
            if (this.selectedCep && this.selectedCep != null) {
                let cepFormmated = this.selectedCep.replace(/-/g, '')
                if (cepFormmated.length === 9) {
                    alert('CEP inexistente! Usar CEP no formato 99999-999 ou apenas 8 dígitos!')
                    cep.focus()
                    return false
                }
                else if (!validaCep.test(cepFormmated)) {
                    alert('Formato inválido! CEP no formato 99999-999 ou apenas números!')
                    this.selectedCep = ''
                    cep.focus()
                    return false
                }
                fetch(`https://viacep.com.br/ws//${this.selectedCep}/json/`)
                    .then((response) => {
                        response.json().then(data => {
                            if (!('erro' in data)) {
                                /* Reorder array keys */
                                const newData = {
                                    'logradouro': null,
                                    'complemento': null,
                                    'bairro': null,
                                    'localidade': null,
                                    'uf': null,
                                    'cep': null,
                                    'ibge': null,
                                    'unidade': null,
                                    'gia': null,
                                    'ddd': null,
                                    'siafi': null
                                }
                                data = Object.assign(newData, data)
                                /* Slice array to get only first 7 elements */
                                this.cepInfo = Object.values(data).slice(0, 7)
                                this.ibgeUrl = 'https://cidades.ibge.gov.br/brasil/' + data['uf'].toLowerCase() + '/' + data['localidade'].toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').replace(/ /g, '-')
                                this.t2Display = 'block'
                                //console.log(this.cepInfo)
                            }
                            else {
                                alert('CEP não encontrado!')
                                cep.focus()
                            }
                        })
                    })
                    .catch((err) => {
                        alert('CEP com formato inválido!')
                        cep.focus()
                        //console.error(err)
                    })
            }
        },
        limpaCep() {
            this.t2Display = 'none'
            this.selectedCep = null
            this.ibgeUrl = null
            this.cepInfo = []
            this.$refs.cepForm.resetValidation()
        },
    },
}
</script>