<template>
    <div class="container">

        <div class="card">
            <div class="card-header">
                Empleados
            </div>
            <div class="card-body">
                <div class="form-group">
                  <label for="">Nombre</label>
                  <input v-on:input="filtrar()" v-model="txtFiltrar"  type="text" name="" id="" class="form-control" placeholder="" aria-describedby="helpId">
        
                </div>
                <table class="table table-sm">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Correo</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
           
                        
                        <tr v-for="empleado in empleados" :key="empleado.id">
                            <td>{{empleado.id}}</td>
                            <td>{{empleado.nombre}}</td>
                            <td>{{empleado.correo}}</td>
                            <td>
                                <div class="btn-group" role="group" aria-label="">
                                    <div class="row">
                                        <div class="col">
                                            <router-link :to="{name:'Editar',params:{id:empleado.id}}" class="btn btn-info">Editar</router-link>
                                        </div>
                                        <div class="col">
                                            <button type="button" v-on:click="borrarEmpleado(empleado.id)" class="btn btn-danger">Borrar</button>
                                        </div>
                                    </div> 
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="card-footer text-muted">
                Estudio Software
            </div>
        </div>


    </div>
</template>
<script>
import axios from "axios";
export default {

    data(){
        return{
            empleados:[],
            txtFiltrar:'',

        }
    },
    created:function(){
        this.consultarEmpleados();
    },
    methods:{
        filtrar(){
            if (this.txtFiltrar==='') {
                this.consultarEmpleados();
            }

            this.empleados = this.empleados.filter(e =>{

                if (e.nombre.search(this.txtFiltrar) != -1) {
                   return true
                } else {
                   return false
                }

            })
        },
        consultarEmpleados(){
            
            axios.get("https://www.svr1.ar/empleados/").then((result) => {
                console.log(result.data);
                this.empleados = []
                this.empleados = result.data;
  
            })
            
            
            /*fetch('https://www.svr1.ar/empleados/')
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta)=>{
                //console.log(datosRespuesta)
                this.empleados = []
                if(typeof datosRespuesta[0].success==='undefined')
                {
                    this.empleados = datosRespuesta;
                }
            })
            .catch(console.log)*/
        },
        borrarEmpleado(id){
            console.log(id);

            axios.get('https://www.svr1.ar/empleados/?borrar='+id).then((result) => {
                console.log(result.data);
                this.consultarEmpleados()

            })


            /* fetch('https://www.svr1.ar/empleados/?borrar='+id)
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta)=>{
                console.log(datosRespuesta)
                window.location.href="listar"
            })
            .catch(console.log)*/
        }

    }
}
</script>
