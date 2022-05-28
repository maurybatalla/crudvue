<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Editar Empleados
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="actualizarRegistro">
                    <div class="form-group">
                      <label for="">Nombre: </label>
                      <input type="text"
                        class="form-control" name="nombre" required v-model="empleado.nombre" id="nombre" aria-describedby="helpId" placeholder="">
                      <small id="helpId" class="form-text text-muted">Ingrese el nombre del empleado</small>
                    </div>
                     <div class="form-group">
                      <label for="">Correo: </label>
                      <input type="email"
                        class="form-control" name="correo" required v-model="empleado.correo" id="correo" aria-describedby="helpId" placeholder="">
                      <small id="helpId" class="form-text text-muted">Ingrese Email</small>
                    </div>

                    <div class="btn-group" role="group" aria-label="">
                        <button type="submit" class="btn btn-success">Modificar </button>
                        <router-link :to="{name:'Listar'}" class="btn btn-warning">Cancelar</router-link>
                       
                    </div>

                </form>
            </div>
            <div class="card-footer text-muted">
                Estudio Software
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            empleado:{}
        }
    },
    created:function(){
        this.obtenerInformacionID()
    },
    methods:{
        obtenerInformacionID(){
            fetch('https://www.svr1.ar/empleados/?consultar='+this.$route.params.id)
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta)=>{
                console.log(datosRespuesta)
                this.empleado = datosRespuesta[0]
                console.log(this.empleado)
              
            })
            .catch(console.log)
        },
        actualizarRegistro(){
            var datosEnviar={id:this.$route.params.id,nombre:this.empleado.nombre,correo:this.empleado.correo}

             fetch('https://www.svr1.ar/empleados/?actualizar='+this.$route.params.id,{
                 method:"POST",
                 body:JSON.stringify(datosEnviar)
             })
            .then(respuesta=>respuesta.json())
            .then((datosRespuesta=>{
                console.log(datosRespuesta);
                window.location.href='../listar'
            }))
        },
    }
    
}
</script>
