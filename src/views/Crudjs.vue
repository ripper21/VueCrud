<template>
  <v-container grid-list-xl>
    <v-layout row wrap>
      <v-flex md6>

        <v-card class="mb-3" v-for="(item, index) in listaTareas" :key="index">
          <v-card-text>
            <v-chip label color="blue" text-color="white">
              <v-icon left> mdi-comment-outline</v-icon>
              {{item.titulo}}
            </v-chip>
            <p>{{item.descripcion}}</p>
            <v-btn class="mr-3 ml-0" color="warning" @click="editar (index)">Editar</v-btn>
            <v-btn class="ma-3" color="error" @click="eliminarTarea(item.id)">Eliminar</v-btn>
          </v-card-text>
        </v-card>

      </v-flex>

      <v-flex md6 v-if="formAgregar">
        <v-card class="mb-3 pa-3">
          <v-form @submit.prevent="agregarTarea">
            <v-text-field label="titulo de la tarea" v-model="titulo"></v-text-field>
            <v-textarea label="Descripcion de tarea" v-model="descripcion"></v-textarea>
            <v-btn block color="success" type="submit">Agreagar tarea</v-btn>
          </v-form>
        </v-card>
      </v-flex>

            <v-flex md6 v-if="!formAgregar">
        <v-card class="mb-3 pa-3">
          <v-form @submit.prevent="editarTarea">
            <v-text-field label="titulos de la tarea" v-model="titulo"></v-text-field>
            <v-textarea label="Descripcion de tarea" v-model="descripcion"></v-textarea>
            <v-btn block color="warning" type="submit">Editar Tarea</v-btn>
          </v-form>
        </v-card>
      </v-flex>

    </v-layout>
    <v-snackbar v-model="snackbar">
    {{mensaje}}
    <v-btn  class="center" color="blue" flat @click="snackbar = false">
    Cerrar
    </v-btn>
     </v-snackbar>

  </v-container>
</template>

<script>
export default {
    data() {
        return {
            listaTareas: [
                {id:1, titulo: 'titulo tarea 1', descripcion: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Expedita autem modi sint suscipit necessitatibus'},       
                 {id:2, titulo: 'titulo tarea 2', descripcion: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Expedita autem modi sint suscipit necessitatibus'},           
                 {id:3, titulo: 'titulo tarea 3', descripcion: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Expedita autem modi sint suscipit necessitatibus'}
            ],
            titulo: '',
            descripcion: '',
            snackbar: false,
            mensaje:'',
            formAgregar: true,
            indexTarea:''

        }
        
    },

    methods:{
        agregarTarea () {
           // console.log(this.titulo, this.descripcion);
            if(this.titulo === '' || this.descripcion=== '' ){
                this.snackbar = true
                this.mensaje = 'debes llenar los campos vacios'
            }else {
                this.listaTareas.push({
                    id: Date.now(),
                    titulo: this.titulo,
                    descripcion: this.descripcion
                })
                this.titulo = ''
                this.descripcion = ''
                this.snackbar = true
                this.mensaje = 'Tarea Agregada'
                 }
            },
            eliminarTarea(id){
                this.listaTareas = this.listaTareas.filter(e => e.id != id )
            },
            editar(index){
                this.formAgregar = false
                this.titulo = this.listaTareas[index].titulo
                this.descripcion = this.listaTareas[index].descripcion
                this.indexTarea = index
            },
            editarTarea(){
                this.listaTareas[this.indexTarea].titulo = this.titulo
                this.listaTareas[this.indexTarea].descripcion = this.descripcion
                this.formAgregar = true
                this.titulo = ''
                this.descripcion = ''
                this.snackbar = true
                this.mensaje = ' Tarea editada Exitosamente '
            }
        }
    }
</script>