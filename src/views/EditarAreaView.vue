<template>
    <div class="container">
        <div class="abs-center">
            
            <form action="" @submit.prevent="guardar()">
                <div class="mb-3">
                    <label class="form-label">Nombre Area</label>
                    <input type="text" v-model="area.nombreArea" class="form-control">
                </div>
                <label class="form-label">Nombre Responsable</label>
                <textarea style="width: 100%" v-model="area.nombreEncargado"></textarea>
                <label class="form-label">Numero de Funcionarios</label>
                <textarea style="width: 100%" v-model="area.numFuncionarios"></textarea>
                <button type="submit" class="btn btn-primary m-2">Guardar</button>
                <button class="btn btn-light m-2">Cancelar</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'editarAreaView',
    data() {
        return {
            area:{
                nombreArea: null,
                nombreEncargado: null,
                numFuncionarios: null
            }
        }
    },
    methods: {
        getArea(){
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/areas/"+this.$route.params.id
            })
            .then(response => {
                this.area = response.data;
            console.log(response);
            })
            .catch(e => console.log(e));
        },
        guardar(){
            axios({
                method: "patch",
                url: process.env.VUE_APP_RUTA_API+"/areas/"+this.$route.params.id,
                data: this.area
            })
            .then(response => {
                this.$store.state.mensaje = {
                    texto: "El activo se edito exitosamene",
                    tipo: "exito"
                };
                this.$store.dispatch('addMensajeAction');
                this.$router.push({name: 'areas'});
            })
            .catch(e => console.log(e));
        }
    },
    computed: {
    },
    mounted() {
        this.getArea()
    },
    components: {
    }
}
</script>

<style scoped>
form {
    max-width: 400px;
}
</style>