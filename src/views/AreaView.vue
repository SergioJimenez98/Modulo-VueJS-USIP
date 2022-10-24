<template>
    <div>
        <h1>areas</h1>
        <form @submit.prevent="agregarArea()">
            <div class="input-group mb-3">
                <input type="text" class="form-control" v-model="area.nombreArea" placeholder="Nueva Area"
                    aria-describedby="button-addon2">
                <button class="btn btn-outline-secondary" type="submit">Agregar</button>
            </div>
        </form>
        <h5>Buscador de areas {{textoABuscar}}</h5>
        <form action="">
        <div class="input-group mb-3">
            <input type="text" v-model="textoABuscar" class="form-control" placeholder="Buscar areas" >
            <button class="btn btn-outline-secondary" @click.prevent="getAreas()">Buscar</button>
        </div>
       </form> 

       <!--<div class="form-check form-switch">
            <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckChecked"
                :checked="$store.state.soloTerminado" v-on:input="$store.state.soloTerminado = $event.target.checked">
            <label class="form-check-label" for="flexSwitchCheckChecked">Mostrar solo terminados
                {{$store.state.soloTerminado}}</label>
        </div>-->

        <div class="accordion" id="accordionExample">

            <div class="accordion-item" v-for="area in areas">
                <h2 class="accordion-header" id="headingTwo">
                    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
                        :data-bs-target="`#collapseTwo${area.id}`" aria-expanded="false"
                        :aria-controls="`collapseTwo${area.id}`">
                        <!--<input type="checkbox" :checked="value.terminado"
                            @click="setearCheckbox(value.terminado, value.id)">-->
                        {{area.nombreArea}}
                    </button>
                </h2>
                <div :id="`collapseTwo${area.id}`" class="accordion-collapse collapse" aria-labelledby="headingTwo"
                    data-bs-parent="#accordionExample">
                    <div class="accordion-body"> 
                        Encargado: {{area.nombreEncargado}} - 
                        Numero de funcionarios: {{area.numFuncionarios}}
                        <app-acciones @onAccion="irA($event, area.id)"></app-acciones>

                    </div>
                </div>
            </div>

        </div>


    </div>
</template>

<script>
import AppAcciones from '@/components/AppAcciones.vue';
export default {
    name: 'areaView',
    data() {
        return {
            //soloTeminado: false,
            textoABuscar: '',
            area: {
                nombreArea: null,
                nombreEncargado: null,
                numFuncionarios: null
            },
            areas: []
        }
    },
    methods: {
        agregarArea() {
            axios({
                method: "post",
                url: process.env.VUE_APP_RUTA_API+"/areas",
                data: this.area
            })
                .then(response => {
                    console.log(response);
                    this.area.nombreArea = null;
                    this.getAreas();
                })
                .catch(e => console.log(e));
        },
        getAreas() {
            axios({
                method: "get",
                url: process.env.VUE_APP_RUTA_API+"/areas/?q="+this.textoABuscar
            })
                .then(response => {
                    this.areas = response.data;
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        setearCheckbox(terminado, id) {
            axios({
                method: "patch",
                url: process.env.VUE_APP_RUTA_API+"/areas/" + id,
                data: {
                    terminado: !terminado
                }
            })
                .then(response => {
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        irA(opcion, area_id) {
            if (opcion === 'editar') {
                this.$router.push({ name: 'editarArea', params: { id: area_id } });
            } else {
                if (confirm("Esta seguro de eliminar area")) {
                    axios({
                        method: "delete",
                        url: process.env.VUE_APP_RUTA_API+"/areas/" + area_id
                    })
                        .then(response => {
                            this.getAreas();
                            console.log(response);
                        })
                        .catch(e => console.log(e));
                }
            }
        },
        lista_(){
            
            if(this.$store.state.soloTerminado){
                return this.areas.filter(item =>{
                    return item.terminado;
                });
            }
            return this.areas;
        }
    },
    computed: {
        lista(){
            if(this.$store.state.soloTerminado){
                return this.areas.filter(item =>{
                    return item.terminado;
                });
            }
            return this.areas;
        }
    },
    mounted() {
        this.getAreas()
    },
    components: {
        AppAcciones
    }
}
</script>

<style>

</style>