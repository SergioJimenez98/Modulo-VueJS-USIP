<template>
    <div class="container">
        <div class="abs-center">
            
            <form action="" @submit.prevent="guardar()">
                <div class="mb-3">
                    <label class="form-label">Clasificacion</label>
                    <input type="text" v-model="activo.clasificacion" class="form-control">
                </div>
                <label class="form-label">Marca</label>
                <textarea style="width: 100%" v-model="activo.marca"></textarea>
                <label class="form-label">Modelo</label>
                <textarea style="width: 100%" v-model="activo.modelo"></textarea>
                <label class="form-label">Estado</label>
                <select style="width: 100%" v-model="activo.estado">
                    <option value="nuevo">nuevo</option>
                    <option value="usado">usado</option>
                    <option value="desuso">desuso</option>
                </select>
                <label class="form-label">Area Correspondiente</label>
                <select style="width: 100%" v-model="activo.areaNombre">
                    <option v-for="area in areas" :value="area.nombreArea">{{area.nombreArea}}</option>
                </select>
                <button type="submit" class="btn btn-primary m-2">Guardar</button>
                <button class="btn btn-light m-2">Cancelar</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'editarActivoView',
    data() {
        return {
            activo:{
                clasificacion: null,
                marca: null
            },
            areas: {}
        }
    },
    methods: {
        getActivo(){
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/activos/"+this.$route.params.id
            })
            .then(response => {
                this.activo = response.data;
            console.log(response);
            })
            .catch(e => console.log(e));
        },
        getArea(){
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/areas"
            })
            .then(response => {
                this.areas = response.data;
            console.log("AREAS" + Object.values(response.data));
            })
            .catch(e => console.log(e));
        },
        guardar(){            
            axios({
                method: "patch",
                url: process.env.VUE_APP_RUTA_API+"/activos/"+this.$route.params.id,
                data: this.activo
            })
            .then(response => {              
                this.$store.state.mensaje = {
                    texto: "El activo se edito exitosamene",
                    tipo: "exito"
                };
                this.$store.dispatch('addMensajeAction');
                this.$router.push({name: 'activos'});
            })
            .catch(e => console.log(e));
        }
    },
    computed: {
    },
    mounted() {
        this.getActivo(),
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