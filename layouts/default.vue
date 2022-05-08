<template>
    <v-app dark>
        <v-app-bar
            app
            color="deep-purple accent-4"
            dense
            dark
            fixed
        >
            <v-toolbar-title>
                <NuxtLink class="link" to="/">
                    {{ titulo }}
                </NuxtLink>
            </v-toolbar-title>

            <v-spacer />

            <v-btn
                v-for="(botao, idx) in botoesFiltrados"
                :key="idx"
                :color="botao.color"
                :to="botao.to"
                class="mx-2"
            >
                {{ botao.texto }}
            </v-btn>
        </v-app-bar>
        <v-main>
            <v-container>
                <Nuxt />
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
export default {
    data () {
        return {
            titulo: 'My Account',
            botoes: [
                {
                    texto: 'Login',
                    color: 'green',
                    to: '/login'
                },
                {
                    texto: 'Criar Conta',
                    color: 'light blue',
                    to: '/criar-conta'
                }
            ],
            pagina: null
        }
    },
    computed: {
        botoesFiltrados() {
            let btns = [];
            if(this.$route.name == 'login') {
                btns.push(this.botoes[1])

                return btns
            }
            if(this.$route.name == 'criar-conta') {
                btns.push(this.botoes[0])

                return btns
            }

            return this.botoes
        }
    }
}
</script>

<style scoped>
.link {
    text-decoration: none;
    color: #fff;
}
</style>
