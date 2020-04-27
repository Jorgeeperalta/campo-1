<template>
  <v-card>
    <div class="text-center">
      <v-snackbar v-model="snackbar" :timeout="timeout" color="success" top
        >{{ text }}
        <v-btn color="success" text @click="snackbar = false">Close </v-btn>
      </v-snackbar>
    </div>
    <v-text-field
      v-model="search"
      append-icon="mdi-magnify"
      label="Buscar"
      single-line
      hide-details
      v-if="dialog2 == false"
    ></v-text-field>

    <v-data-table
      :headers="cabeceras"
      :items="educacion"
      :search="search"
      class="elevation-1 mytable"
      v-if="dialog2 == false"
    >
      <template v-slot:top>
        <v-toolbar
          src="https://dhb3yazwboecu.cloudfront.net/270/fabercastell/colores/154_l.jpg"
        >
          <h1>CALCULO DE AREAS</h1>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>

          <v-dialog v-model="dialog" max-width="800px">
            <template v-slot:activator="{ on }">
              <v-btn @click="reset" v-on="on" text>Agrega areas</v-btn>
            </template>
            <v-card>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>
                <v-card-text>
                  <v-row>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-text-field
                        v-model="editedItem.name_campo"
                        :rules="nameRules"
                        label="Nombre del Establecimiento"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-select
                        v-model="editedItem.provincias"
                        :search="search"
                        item-value="value"
                        :items="provincias"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Seleccione una Provincia"
                        required
                        >{{ provincias }}</v-select
                      >
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-select
                        v-model="editedItem.partidos"
                        :search="search"
                        item-value="value"
                        :items="partidos"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Seleccione una Partido"
                        required
                        >{{ partidos }}</v-select
                      >
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-text-field
                        v-model="editedItem.direccion"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Ingrese direccion del establecimiento"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-select
                        v-model="editedItem.clasificacion"
                        :search="search"
                        item-value="value"
                        :items="clasificacion"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Clasificacion bobina"
                        required
                        >{{ clasificacion }}</v-select
                      >
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-text-field
                        v-model="editedItem.nom_area"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Ingrese nombre del Area"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col class="col-12" v-else-if="bandera == false">
                      <v-text-field
                        v-model="editedItem.nom_area"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Ingrese nombre del Area"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col class="col-12">
                      <v-select
                        v-model="editedItem.nivel"
                        :search="search"
                        item-value="value"
                        :items="nivelesDisponibles"
                        :rules="[v => !!v || 'Dato Requerido']"
                        label="Tipo de ambiente"
                        required
                        >{{ nivelesDisponibles }}</v-select
                      >
                    </v-col>
                    <v-col class="col-6" v-if="bandera == true">
                      <v-text-field
                        v-model="editedItem.hectarea"
                        v-on:keyup.enter="validate"
                        :rules="nameRules"
                        label="Ingrese cantidad de Hectareas"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col class="col-12" v-else-if="bandera == false">
                      <v-text-field
                        v-model="editedItem.hectarea"
                        v-on:keyup.enter="validate"
                        :rules="nameRules"
                        label="Ingrese cantidad de Hectareas"
                        required
                      ></v-text-field>
                    </v-col>

                    <v-col class="col-6" v-if="bandera == true">
                      <template>
                        <v-menu
                          ref="menu"
                          v-model="menu"
                          :close-on-content-click="false"
                          transition="scale-transition"
                          offset-y
                          min-width="290px"
                        >
                          <template v-slot:activator="{ on }">
                            <v-text-field
                              v-model="editedItem.fecha_cap"
                              label="Fecha"
                              prepend-icon="mdi-calendar"
                              readonly
                              :rules="[v => !!v || 'Dato Requerido']"
                              v-on:keyup.enter="validate"
                              v-on="on"
                            ></v-text-field>
                          </template>
                          <v-date-picker
                            ref="picker"
                            v-model="fecha"
                            locale="es-es"
                            :max="new Date().toISOString().substr(0, 10)"
                            min="1900-01-01"
                            @change="
                              editedItem.fecha_cap = parseDate(fecha);
                              save();
                            "
                          ></v-date-picker>
                        </v-menu>
                      </template>
                    </v-col>
                  </v-row>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="#EE2F5A" class="mr-4" @click="close"
                    >Cancelar
                  </v-btn>
                  <v-btn
                    :disabled="!valid"
                    color="#06A0A2"
                    class="mr-4"
                    @click="validate"
                  >
                    Guardar
                  </v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>

      <template v-slot:item.action="{ item }">
        <v-card-actions>
          <v-btn color="lime lighten-3" @click="editItem(item)">
            editar
          </v-btn>

          <v-btn color="deep-orange lighten-3" @click="deleteItem(item)">
            eliminar
          </v-btn>
        </v-card-actions>
      </template>

      <template v-slot:item.estado="{ item }">
        {{ item.estado ? "Completo" : "incompleto" }}
      </template>
    </v-data-table>
    <div v-if="aux_pdf == false">
      <iframe src="http://localhost:8081/" width="100%" height="400px"></iframe>
    </div>

    <template>
      <v-row justify="center">
        <v-dialog
          v-model="dialog2"
          hide-overlay
          transition="dialog-bottom-transition"
        >
          <template v-slot:activator="{ on }">
            <v-btn
              block
              color="green"
              text
              small
              dark
              v-on="on"
              v-if="dialog2 == false"
              @click="(aux_pdf = true)"
              >Generar informe</v-btn
            >
          </template>
          <v-card>
            <v-toolbar dark color="grey">
              <v-btn icon dark @click="dialog2 = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title>Informe</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-toolbar-items>
                <v-btn dark text @click="(dialog2 = false), calcular_ambiente, exportPDF(salida)"
                  >Generar pdf</v-btn
                >
              </v-toolbar-items>
            </v-toolbar>
            <v-list three-line subheader>
              <div style="display: flex; justify-content: space-around">
                <h1>Establecimiento {{ aux }}</h1>
              </div>
              <div style="display: flex; justify-content: space-around">
                <h3>Provincia {{ aux_pro }}</h3>
              </div>
              <div style="display: flex; justify-content: space-around">
                <h3>Partido {{ aux_par }}</h3>
              </div>
              <div style="display: flex; justify-content: space-around">
                <h3>Direccion {{ aux_dir }}</h3>
              </div>
              <div>
                <v-subheader>Fecha de Relevamiento {{ aux_fecha }}</v-subheader>
              </div>

              <v-list-item v-for="(item, index) in salida" :key="index">
                <v-list-item-content>
                  <div v-if="contador == '0'">
                    <v-list-item-title
                      >Usuarios:{{ item.vacas }}</v-list-item-title
                    >
                  </div>

                  <v-list-item-title
                    >Potrero :{{ item.area }}</v-list-item-title
                  >
                  <v-list-item-title
                    >Tipo de ambiente:{{ item.campo }}
                  </v-list-item-title>
                  <v-list-item-title
                    >Caracteristica del ganado: {{ item.clasi }}
                  </v-list-item-title>
                  <v-list-item-title
                    >Cantidad de cabezas: {{ item.vacas }}
                  </v-list-item-title>
                  <v-list-item-subtitle
                    >Tiempo recomendado de Pastura es de
                    {{ item.pastura }} dias</v-list-item-subtitle
                  >
                  <v-list-item-subtitle
                    >Tiempo recomendado de Descanzo es de
                    {{ item.descanzo }} dias</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title
                    >desde...{{ aux_fecha }} hasta:...</v-list-item-title
                  >
                  <v-list-item-subtitle
                    >********************************</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
            </v-list>
            <v-divider></v-divider>
            <v-list three-line subheader>
              <v-subheader>General</v-subheader>
              <!--  <v-list-item>
                <v-list-item-action>
                  <v-checkbox v-model="notifications"></v-checkbox>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>Notifications</v-list-item-title>
                  <v-list-item-subtitle
                    >************************</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-action>
                  <v-checkbox v-model="sound"></v-checkbox>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>******************</v-list-item-title>
                  <v-list-item-subtitle
                    ></v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item> -->
              <!--     <v-list-item>
               <v-list-item-action>
                  <v-checkbox v-model="widgets"></v-checkbox>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>************</v-list-item-title>
                  <v-list-item-subtitle
                    >**************************</v-list-item-subtitle
                  >
                </v-list-item-content>
                 <iframe src="http://localhost:8082/" width="100%" height="500px"></iframe>
              </v-list-item> -->
            </v-list>
            <Mapa></Mapa>
            <div v-for="(item, index) in datas" :key="index">
              {{ item.valor }}
            </div>
          </v-card>
        </v-dialog>
      </v-row>
    </template>
    <div align="right">
      <v-btn color="red" text>
        Cerrar
      </v-btn>
    </div>

    <!--    <h1>{{salida}}</h1>  -->
  </v-card>
</template>

<script>
import jsPDF from 'jspdf';
import 'jspdf-autotable'
import Mapa from "@/components/mapa.vue";
import axios from "axios";
export default {
  components: {
    Mapa
  },
  data: () => ({
    aux_pdf: false,
    aux_provincias: "",
    aux_partido: "",
    aux_direccion: "",
    aux_pro: "",
    aux_par: "",
    aux_dir: "",
    aux_clasificacion: "",
    datas: "",
    aux_fecha: "",
    aux1: 0,
    fecha_oto: "03-21",
    fecha_inv: "06-21",
    fecha_pri: "09-21",
    fecha_ver: "12-21",
    aux_hectarea: "",
    contador: 0,
    //date: new Date().toISOString().substr(0, 10),
    fecha: "",
    menu: false,
    bandera: true,
    educacion: [],
    salida: [],
    dialog: false,
    dialog2: false,
    notifications: false,
    nombre_campo: "",
    nombre_estacion: "",
    nombre_pasto: "",
    sound: true,
    widgets: false,
    snackbar: false,
    text: "Agregado con exito",
    timeout: 2000,
    search: "",
    valid: true,
    hectarea: "",
    selected: [],
    nameRules: [v => !!v || "EL nombre es un dato requerido"],
    select: null,
    clasificacion: ["Ternero", "Novillo", "Vaca", "Toro"],
    provincias: ["Buenos Aires", "Entre Rios", "Cordoba"],
    partidos: ["Lavalle", "Lezama", "Castelli", "Pila"],
    nivel: ["Loma", "Media Loma", "Bajo Dulce", "Alcalinos"],
    nivelesCompletados: [],
    checkbox: false,
    cabeceras: [
      {
        text: "Establecimiento",
        value: "name_campo",
        align: "left",
        sortable: false
      },
      { text: "Nombre del Area", value: "nom_area" },
      { text: "Tipo de pastura", value: "nivel" },
      { text: "Hectareas", value: "hectarea" },
      //  { text: "Altura", value: "titulo" },
      { text: "Fecha de relevamiento", value: "fecha_cap" },
      { text: "Botones", value: "action", sortable: false }
    ],

    editedIndex: -1,
    editedItem: {
      name_campo: "",
      provincias: "",
      partidos: "",
      direccion: "",
      clasificacion: "",
      nom_area: "",
      nivel: "",
      hectarea: 0,
      fecha_cap: ""
    },
    defaultItem: {
      name_campo: "",
      provincias: "",
      partidos: "",
      direccion: "",
      clasificacion: "",
      nom_area: "",
      nivel: "",
      hectarea: 0,
      fecha_cap: ""
    },
    datosSalida: {
      nombre_camp:"",
      area: "",
      vacas: "",
      campo: "",
      pastura: "",
      descanzo: "",
      prov: "",
      partid: "",
      dire: "",
      clasi: ""
    }
  }),
  computed: {
    formTitle() {
      this.activa();
      return this.editedIndex === -1 ? "Ingrese datos" : "Editar";
    },
    nivelesDisponibles() {
      return this.nivel.filter((el, i) => {
        return this.nivelesCompletados.indexOf(i) == -1;
      });
    }
  },
  mounted() {},
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = "YEAR"));
    },

    dialog(val) {
      //   this.editItem.hectarea=this.aux1;

      if (val) {
        if (this.editedIndex < -1) {
          this.reset();
        }
      } else {
        this.close();
      }
    }
  },
  methods: {
  
    exportPDF(sali) {
    
        let pdfName = 'Informe'; 
        var imgData = 'data:image/png;base64,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'
        var doc = new jsPDF();
           
           doc.addImage(imgData, 'png', 155, 0, 50, 40)
           doc.rect(15, 10, 180, 285);
           doc.setFont('times');
           doc.setFontType('bold');
           doc.setFontSize(33);
           doc.rect(15, 10, 180, 60);
           doc.text(20, 20, ' '+ sali[0].nombre_camp +' '); 
           doc.setLineWidth(0.5);
           doc.line(15, 21,160, 21);
           doc.setFont('times');
           doc.rect(160, 10, 35, 25);
           doc.setFontSize(23);
           doc.text(20, 40, 'Provincia: '+ sali[0].prov); 
           doc.setFont('times');
       
           doc.setFontSize(23);         
           doc.text(20, 50, 'Partido: '+ sali[0].partid);  
           doc.text(20, 60, 'Direccion: '+ sali[0].dire); 
           let cont=80;
           doc.setFontSize(13);  
           sali.forEach(element =>{  
                  doc.text(20, cont, 'Potrero: '+ element.area);  
                  cont=cont+10;
                  doc.text(20, cont, 'Tipo de Ambiente: '+ element.campo);
                  cont=cont+10;
                  doc.text(20, cont, 'Caracteristica del ganado: '+ element.clasi);  
                  cont=cont+10;
                  doc.text(20, cont, 'Cantidad de cabezas: '+ element.vacas);
                  doc.setLineWidth(1);
                  cont=cont+5;
                  doc.line(20, cont,150, cont);
                  cont=cont+20;
           })
        doc.save(pdfName + '.pdf');
    },
    activa() {
      fetch("http://localhost:81/S.Final/leaflet-measure/backend/euro")
        .then(datos => datos.json())
        .then(datos => {
          datos.forEach(element => {
            this.aux1 = element.nombre;
          });
        });

      console.log("forrr   " + this.aux1);
      this.editItem.hectarea = this.aux1;
    },

    calcular_ambiente(nom_camp,estacion, hectarea_aux, are, pro, par, dir, clas) {
      let aux;
      fetch("http://localhost:81/S.Final/leaflet-measure/backend/euro")
        .then(datos => datos.json())
        .then(datos => {
          datos.forEach(element => {
            // console.log("este valor  del area en calcular ambiente "+element.nombre);
            aux = element.nombre;
          });
          this.datas = datos;
          // console.log(datos[datos.length-1].nombre)
          this.datas = datos;
        });

      let arrayFecha = this.fecha.split(["-"]);
      let ddmm = arrayFecha[1] + "-" + arrayFecha[2];
      this.aux_fecha =
        arrayFecha[2] + "-" + arrayFecha[1] + "-" + arrayFecha[0];

      console.log(ddmm);
      console.log(ddmm);

      switch (estacion) {
        case "Loma":
          if (ddmm < this.fecha_oto) {
            //  verano  = 1
            //  console.log("console "+ddmm);
            // alert(ddmm+" esta es verano   "+ estacion);
          } else if (ddmm > this.fecha_oto && ddmm < this.fecha_inv) {
            // otonio = 2
            alert(ddmm + " esta es una otonio   " + estacion);
          } else if (ddmm > this.fecha_inv && ddmm < this.fecha_pri) {
            // invierno   = 3
            this.calcular_salida(
              nom_camp,
              hectarea_aux,
              estacion,
              are,
              300,
              120,
              pro,
              par,
              dir,
              clas
            );
            ///////////////
            alert(ddmm + " esta es una invierno   " + estacion);
          } else if (ddmm < this.fecha_ver && ddmm > this.fecha_pri) {
            // primavera = 4
            alert(ddmm + " esta es una prueba primavera   " + estacion);
          }
          break;
        case "Media Loma":
          if (ddmm > this.fecha_ver) {
            //  alert(ddmm+" esta es verano media  "+ estacion);
            alert(nom_camp+ "   nombre campo"),
            this.calcular_salida(
              nom_camp,
              hectarea_aux,
              estacion,
              are,
              500,
              53,
              pro,
              par,
              dir,
              clas
            );
          } else if (ddmm < this.fecha_inv && ddmm > this.fecha_oto) {
            alert(ddmm + " esta es una otonio   " + estacion);
          } else if (ddmm < this.fecha_pri && ddmm > this.fecha_inv) {
            alert(ddmm + " esta es una invierno   " + estacion);
          } else if (ddmm < this.fecha_ver && ddmm > this.fecha_pri) {
            alert(ddmm + " esta es una prueba primavera   " + estacion);
          }
          break;
        case "Bajo Dulce":
          if (ddmm < this.fecha_oto && ddmm > this.fecha_ver) {
            alert(ddmm + " esta es verano  dulce " + estacion);
          } else if (ddmm < this.fecha_inv && ddmm > this.fecha_oto) {
            alert(ddmm + " esta es una otonio   " + estacion);
          } else if (ddmm < this.fecha_pri && ddmm > this.fecha_inv) {
            alert(ddmm + " esta es una invierno   " + estacion);
          } else if (ddmm < this.fecha_ver && ddmm > this.fecha_pri) {
            alert(ddmm + " esta es una prueba primavera   " + estacion);
          }
          break;
        case "Alcalinos":
          if (ddmm < this.fecha_oto && ddmm > this.fecha_ver) {
            alert(ddmm + " esta es verano alcalinos  " + estacion);
          } else if (ddmm < this.fecha_inv && ddmm > this.fecha_oto) {
            alert(ddmm + " esta es una otonio   " + estacion);
          } else if (ddmm < this.fecha_pri && ddmm > this.fecha_inv) {
            alert(ddmm + " esta es una invierno   " + estacion);
          } else if (ddmm < this.fecha_ver && ddmm > this.fecha_pri) {
            alert(ddmm + " esta es una prueba primavera   " + estacion);
          }
          break;
      }
    },
    calcular_salida(
      nom_cam,
      hectarea,
      ambiente,
      area,
      bandera_epoca,
      disp,
      pr,
      pa,
      di,
      cla
    ) {
      
      let dia;
      let descan;
      let disponibilidad = bandera_epoca;
      let total_disp = "";
      var total;

      total_disp = hectarea * disponibilidad;
      total = total_disp / disp;
      if (bandera_epoca === 500) {
        total = Math.round(total);
        dia = 5;
        descan = 35;
      } else if (bandera_epoca === 300) {
        total = Math.round(total * 1.6666666667);
        dia = 10;
        descan = 75;
      }
            console.log(nom_cam),
      this.datosSalida = {
        nombre_camp:nom_cam,
        area: area,
        vacas: total,
        campo: ambiente,
        pastura: dia,
        descanzo: descan,
        prov: pr,
        partid: pa,
        dire: di,
        clasi: cla
      };
      this.salida.push(this.datosSalida);
   
    },

    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${month}/${day}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${day.padStart(2, "0")}-${month.padStart(2, "0")}-${year}`;
    },

    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
        if (this.editedIndex > -1) {
          Object.assign(this.educacion[this.editedIndex], this.editedItem);
          this.nivelalcanzado();
        } else {
          this.nivelalcanzado();
          if (this.bandera == true) {
            this.educacion.push(this.editedItem);
            this.nombre_estacion = this.editedItem.fecha_cap;
            this.nombre_campo = this.editedItem.name_campo;
            this.nombre_pasto = this.editedItem.nivel;

            this.aux_provincias = this.editedItem.provincias;
            this.aux_partido = this.editedItem.partidos;
            this.aux_direccion = this.editedItem.direccion;
            this.aux_clasificacion = this.editedItem.clasificacion;

            (this.aux_pro = this.aux_provincias),
              (this.aux_par = this.aux_partido),
              (this.aux_dir = this.aux_direccion),
              (this.aux = this.nombre_campo);

            this.aux_hectarea = this.educacion[this.contador].hectarea;

            this.calcular_ambiente(
              this.nombre_campo,
              this.educacion[this.contador].nivel,
              this.aux_hectarea,
              this.educacion[this.contador].nom_area,
              this.educacion[this.contador].provincias,
              this.educacion[this.contador].partidos,
              this.educacion[this.contador].direccion,
              this.educacion[this.contador].clasificacion
            );
            this.snackbar = true;
            this.bandera = false;
            this.close();
          } else {
            this.contador++;
            this.educacion.push(this.editedItem);

            this.editedItem.name_campo = this.nombre_campo;
            this.editedItem.fecha_cap = this.nombre_estacion;
            this.editedItem.provincias = this.aux_provincias;
            this.editedItem.partidos = this.aux_partido;
            this.editedItem.direccion = this.aux_direccion;
            this.editedItem.clasificacion = this.aux_clasificacion;

            this.aux_hectarea = this.educacion[this.contador].hectarea;

            this.calcular_ambiente(
              this.nombre_campo,
              this.educacion[this.contador].nivel,
              this.aux_hectarea,
              this.educacion[this.contador].nom_area,
              this.educacion[this.contador].provincias,
              this.educacion[this.contador].partidos,
              this.educacion[this.contador].direccion,
              this.educacion[this.contador].clasificacion
            );

            this.snackbar = true;
            this.bandera = false;
            this.close();
          }
        }
      }
    },

    reset() {
      if (this.$refs.form) this.$refs.form.reset();
      this.editedItem.hectarea = this.aux1;
    },
    editItem(item) {
      this.editedItem.nivel = this.nombre_pasto;
      this.editedIndex = this.educacion.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    nivelalcanzado() {
      if (this.editedItem.estado == true) {
        if (this.editedItem.nivel == "Altemanthera filoxeroides") {
          this.nivelesCompletados.push(0);
        } else if (this.editedItem.nivel == "Phalaris angusta") {
          this.nivelesCompletados.push(1);
        } else if (this.editedItem.nivel == "Echinochloa helodes")
          this.nivelesCompletados.push(2);
        else if (this.editedItem.nivel == "Carex riparia")
          this.nivelesCompletados.push(3);
      }
    },
    deleteItem(item) {
      const index = this.educacion.indexOf(item);
      confirm("¿Está seguro que desea eliminarlo?") &&
        this.educacion.splice(index, 1);
    },
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    }
  }
};
</script>

<style>
.v-dialog {
  width: 100%;
  padding: 33px;
}
.v-sheet {
  width: -webkit-fill-available;
}
.color {
  color: "red";
}
.h1,
h1 {
  font-size: 2.5rem;
  font-variant: all-petite-caps;
}
.v-dialog:not(.v-dialog--fullscreen) {
  max-height: 100%;
}
</style>
