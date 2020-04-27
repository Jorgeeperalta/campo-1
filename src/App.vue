<template>
  <v-app>
    <v-app-bar app color="grey" dark>

     

      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="120"
        />

        <h2>Reconocimiento de Ambientes en la cuenca del salado</h2>
      </div>

      <v-spacer></v-spacer>
      
      <v-btn target="_blank" text @click="cambiar2" v-if="variable=='NO'">
        <span class="mr-2">REGISTRARSE</span>
        <v-icon>archive</v-icon>
      </v-btn>

      <v-btn target="_blank" text @click="cambiar" v-if="variable=='NO'">
        <span class="mr-2">LOGIN</span>
        <v-icon>open_in_new</v-icon>
      </v-btn>
      <v-btn target="_blank" text @click="cambiar2" v-if="variable=='SI'">
        <span class="mr-2">Actualizar Perfil</span>
        <v-icon>open_in_new</v-icon>
      </v-btn>
      
    </v-app-bar>

 

            
  <section
    id="welcome"
    class="hide-overflow"
    v-if="estado == 'NO' && estado2 == 'NO'"
  >
    <v-layout>
      <v-flex
        hidden-sm-and-down
        md6
      >
        <v-img
          :src="require('@/assets/elegida2.jpg')"
          height="100vh"
        />
      </v-flex>

      <v-flex
        xs12
        md6
        align-content-space-between
        layout
        :pa-5="$vuetify.breakpoint.smAndDown"
        wrap
      >
        <base-bubble-1
          style="transform: rotate(180deg) translateY(25%)"
        />

        <v-layout
          align-center
          justify-center
        >
          <v-flex
            xs10
            md6
          >
          <br>
          <br> <br>
          <br> <br>
          <br>
            <base-heading><h3 color="blue">Pasos a seguir para el funcionamiento del sistema</h3></base-heading>
            <base-text>
                1.  LOGIN...          usuario: jorge   password: 12345678 <br>
                2.  Cargar tabla con datos del establecimiento, area, tipo de pastura, altura, epoca del año.<br>
                3.  Se repetira el proceso tantas veces como areas a calcular alla.<br>
                4.  Oprimir boton generar informe.<br>
                5.  Se puede descargar pdf.
            </base-text>
            <base-btn class="mt-4">
              
            </base-btn>
          </v-flex>
        </v-layout>

        <base-bubble-2
          style="transform: rotate(180deg) translate(-200px, -15%)"
        />
      </v-flex>
    </v-layout>
  </section>






   

   
    <v-content  v-if="variable=='SI'">
            <calculos />
    </v-content> 

    <v-content v-if="estado=='SI'">
         
           <HelloWorld @estado_login="variable=$event" v-if="variable=='NO'"></HelloWorld>
            
    </v-content>  
     <v-content v-if="estado2=='SI'">
     
           <Registrarse></Registrarse>
        
    </v-content>  


    <v-footer app color="grey" dark>
      <!-- -->
      <h6>Sistema de calculos de ambientes</h6>
    </v-footer>
  </v-app>
</template>

<script>
import HelloWorld from "@/components/HelloWorld";
import calculos from "./components/calculos";
import Registrarse from "@/components/Registrarse";
export default {
  

  components: {
    HelloWorld,
    calculos,
    Registrarse,

  },

  data: () => ({
    estado2:"NO",
    estado: "NO",
    login: "NO",
    variable:'NO',
   
  }),
  methods: {
    cambiar() {
      this.estado = "SI";
      console.log("esta es la variable "+ this.variable);
    },
     cambiar2() {
      this.estado2 = "SI";
    }
  }
};
</script>
<style></style>
