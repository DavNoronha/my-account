<template>
    <v-container>
        <v-row>
            <v-col>
                <h1>Index</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <ul 
                    v-for="(usuario, idx) in usuarios"
                    :key="idx"
                    class="mb-4"
                >
                    <li><h2>{{ usuario.name }}</h2></li>
                    <li>{{ usuario.email }}</li>
                </ul>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            usuarios: {}
        }
    },
    created() {
        this.getUsuarios()
    },
    methods: {
        async getUsuarios() {
            try {
                const {data} = await this.$axios.$get('/users')

                this.usuarios = data;
            } catch(error) {
                console.log(error)
            }
        }
    }
}
</script>
