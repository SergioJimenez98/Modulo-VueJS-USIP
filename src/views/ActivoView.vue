<template>
    <div>
        <h1>activos</h1>
        <form @submit.prevent="agregarActivo()">
            <div class="input-group mb-3">
                <input type="text" class="form-control" v-model="activo.clasificacion" placeholder="Nuevo Activo"
                    aria-describedby="button-addon2">
                <button class="btn btn-outline-secondary" type="submit">Agregar</button>
            </div>
        </form>
        <h5>Buscador de activos {{textoABuscar}}</h5>
        <form action="">
        <div class="input-group mb-3">
            <input type="text" v-model="textoABuscar" class="form-control" placeholder="Buscar activos" >
            <button class="btn btn-outline-secondary" @click.prevent="getActivos()">Buscar</button>
        </div>
       </form> 
       
       <h5>Buscador por estado {{textoABuscar2}}</h5>
        <form action="">
        <div class="input-group mb-3">
            <!--<input type="text" v-model="textoABuscar" class="form-control" placeholder="Buscar por estado" >-->
            <select style="width: 100%" v-model="textoABuscar2">
                    <option value="nuevo">nuevo</option>
                    <option value="usado">usado</option>
                    <option value="desuso">desuso</option>
                </select>
            <button class="btn btn-outline-secondary" @click.prevent="getActivos2()">Buscar</button>
        </div>
       </form> 

       <!--<div class="form-check form-switch">
            <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckChecked"
                :checked="$store.state.soloTerminado" v-on:input="$store.state.soloTerminado = $event.target.checked">
            <label class="form-check-label" for="flexSwitchCheckChecked">Mostrar solo terminados
                {{$store.state.soloTerminado}}</label>
        </div>-->

        <div class="accordion" id="accordionExample">

            <div class="accordion-item" v-for="activo in activos">
                <h2 class="accordion-header" id="headingTwo">
                    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
                        :data-bs-target="`#collapseTwo${activo.id}`" aria-expanded="false"
                        :aria-controls="`collapseTwo${activo.id}`">
                        <!--<input type="checkbox" :checked="value.terminado"
                            @click="setearCheckbox(value.terminado, value.id)">-->
                        {{activo.clasificacion}}
                    </button>
                </h2>
                <div :id="`collapseTwo${activo.id}`" class="accordion-collapse collapse" aria-labelledby="headingTwo"
                    data-bs-parent="#accordionExample">
                    <div class="accordion-body">
                        Marca: {{activo.marca}} -
                        Modelo: {{activo.modelo}} -
                        Estado: {{activo.estado}} -
                        Area Correspondiente: {{activo.areaNombre}}

                        <app-acciones @onAccion="irA($event, activo.id)"></app-acciones>

                    </div>
                </div>
            </div>

        </div>


    </div>
</template>

<script>
import AppAcciones from '@/components/AppAcciones.vue';
export default {
    name: 'activoView',
    data() {
        return {
            //soloTeminado: false,
            textoABuscar: '',
            textoABuscar2: '',
            activo: {
                clasificacion: null,
                modelo: null,
                marca: null,
                estado: null,
            },
            activos: []
        }
    },
    methods: {
        agregarActivo() {
            axios({
                method: "post",
                url: process.env.VUE_APP_RUTA_API+"/activos",
                data: this.activo
            })
                .then(response => {
                    console.log(response);
                    this.activo.clasificacion = null;
                    this.getActivos();
                })
                .catch(e => console.log(e));
        },
        getActivos() {
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/activos/?q="+this.textoABuscar
            })
                .then(response => {
                    this.activos = response.data;
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        getActivos2() {
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/activos/?q="+this.textoABuscar2
            })
                .then(response => {
                    this.activos = response.data;
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        setearCheckbox(terminado, id) {
            axios({
                method: "patch",
                url: process.env.VUE_APP_RUTA_API+"/activos/" + id,
                data: {
                    terminado: !terminado
                }
            })
                .then(response => {
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        irA(opcion, activo_id) {
            if (opcion === 'editar') {
                this.$router.push({ name: 'editarActivo', params: { id: activo_id } });
            } else {
                if (confirm("Esta seguro de eliminar activo")) {
                    axios({
                        method: "delete",
                        url: process.env.VUE_APP_RUTA_API+"/activos/" + activo_id
                    })
                        .then(response => {
                            this.getActivos();
                            console.log(response);
                        })
                        .catch(e => console.log(e));
                }
            }
        },
        lista_(){
            
            if(this.$store.state.soloTerminado){
                return this.activos.filter(item =>{
                    return item.terminado;
                });
            }
            return this.activos;
        }
    },
    computed: {
        lista(){
            if(this.$store.state.soloTerminado){
                return this.activos.filter(item =>{
                    return item.terminado;
                });
            }
            return this.activos;
        }
    },
    mounted() {
        this.getActivos()
    },
    components: {
        AppAcciones
    }
}
</script>

<style>

</style>