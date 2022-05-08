<template>
    <v-container>
        <v-row>
            <v-col>
                <h1>Login</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-card>
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
                    label: 'E-mail',
                    valor: '',
                    erro: false
                },
                {
                    label: 'Senha',
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
                    texto: 'Fazer Login',
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

            if(this.campos.find(campo => campo.erro)) return

            this.campos.forEach(campo => campo.erro = false)
            
            this.login()
        },
        async login() {
            const usuario = {
                email: this.campos[0].valor,
                password: this.campos[1].valor
            }

            try {
                const data = await this.$axios.$get(`/user/login?email=${usuario.email}&pass`)

                console.log('deu bom!')
                
                this.$router.push(`/usuario/${data.name}`)
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
