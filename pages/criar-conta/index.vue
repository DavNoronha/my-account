<template>
    <v-container>
        <v-row>
            <v-col>
                <h1>Criar Conta</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-card
                    rounded
                >
                    <v-row
                        v-for="(campo, idx) in campos"
                        :key="idx"
                        class="mx-5 my-2"
                    >
                        <v-col>
                            <v-text-field
                                v-model="campo.valor"
                                :label="campo.label"
                                outlined
                                :error="campo.erro"
                                @input="limpaErro(idx)"
                            />
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col
                            v-for="(botao, idx) in botoes"
                            :key="idx"
                        >
                            <v-btn
                                :color="botao.color"
                                large
                                @click="botao.acao"
                            >
                                {{ botao.texto }}
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            campos: [
                {
                    label: 'Nome',
                    valor: '',
                    erro: false
                },
                {
                    label: 'E-mail',
                    valor: '',
                    erro: false
                },
                {
                    label: 'Senha',
                    valor: '',
                    erro: false
                },
                {
                    label: 'Confirme sua senha',
                    valor: '',
                    erro: false
                }
            ],
            botoes: [
                {
                    texto: 'Voltar',
                    color: 'orange',
                    acao: () => this.$router.push('/')
                },
                {
                    texto: 'Criar',
                    color: 'light blue',
                    acao: () => this.verificaForm()
                }
            ]
        }
    },
    methods: {
        verificaForm() {
            this.campos.forEach(campo => {
                if(campo.valor == '') campo.erro = true
            })

            if(this.campos[2].valor !== this.campos[3].valor) {
                this.campos[2].erro = true;
                this.campos[3].erro = true;
                return
            }

            if(this.campos.find(campo => campo.erro)) return

            console.log('uai')

            this.campos.forEach(campo => campo.erro = false)
            
            this.criarConta()
        },
        async criarConta() {
            const usuario = {
                name: this.campos[0].valor,
                email: this.campos[1].valor,
                password: this.campos[2].valor
            }

            try {
                await this.$axios.$post('/users', usuario)

                this.$router.push('/')
            } catch(erro) {
                console.log(erro)
            }
        },
        limpaErro(idx) {
            this.campos[idx].erro = false
        }
    }
}
</script>