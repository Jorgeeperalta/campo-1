import Registrarse from '@/components/Registrarse';
<template>
  <div v-if="next == 'NO'">
    <div
      class="h-100 contenedor-foto col-md-12 col"
      style='background: url("/img/fondo2.fd760fc7.jpg") rgb(0, 0, 0);'
    >
      <v-img
        src="https://image.shutterstock.com/image-photo/cows-sunset-la-pampa-argentina-260nw-1218712885.jpg"
        aspect-ratio="1.7"
      >
        <br />
        <v-card class="mx-auto" max-width="400">
            <v-row justify="center">
          <h2 justify="center">Registrarse</h2>
            </v-row>
          <v-parallax height="500">
            <v-row justify="center">
              <v-col class="text-center" cols="12"> </v-col>
            </v-row>
            <template>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="apellido"
                        label="APELLIDO"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col>
                    <br />

                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="nombre"
                        label="NOMBRE"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col>
                    <br />
                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="telefono"
                        label="TELEFONO"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col>
                    <br />
                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="email"
                        label="EMAIL"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col>
                   <!-- <br />
                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="nombre_usuario"
                        label="NOMBRE DE USUARIO"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col> -->
                    <br />

                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="password"
                        :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                        :rules="[rules.required, rules.min]"
                        :type="show1 ? 'text' : 'password'"
                        name="input-10-1"
                        label="INGRESE PASSWORD"
                        hint="At least 8 characters"
                        counter
                        @click:append="show1 = !show1"
                        v-on:keyup.enter="comprobar"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-form>
            </template>
          </v-parallax>

          <v-card-text class="text--primary">
            <div></div>

            <div></div>
          </v-card-text>

          <v-card-actions>
            <v-row>
              <v-dialog v-model="dialog" width="600px">
                <template v-slot:activator="{ on }">
                  <v-btn v-on="on" text>Terminos y condiciones</v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="headline">TÉRMINOS Y CONDICIONES DE USO DE LA PÁGINA WEB</span>
                  </v-card-title>
                  <v-card-text
                    >Esta Web pertenece a la Empresa CUENCA DEL SALADO SA (en adelante
                    LA EMPRESA) y su acceso y utilización está sujeta a la
                    aceptación y cumplimiento de los términos y condiciones que
                    se exponen a continuación: 1. RESPONSABILIDAD LA EMPRESA se
                    reserva el derecho de modificar en forma unilateral, sin
                    mediar previo aviso, ni comunicación al USUARIO y/o
                    VISITANTE, estos Términos y Condiciones, el diseño, la
                    presentación o su configuración, los servicios ofrecidos,
                    los requisitos de registro o de utilización de la página,
                    sin que ello genere derecho a reclamo o indemnización alguna
                    en favor del USUARIO y/o VISITANTE. La utilización de los
                    servicios y/o herramientas digitales, existentes en la
                    página, requerirán del USUARIO y/o VISITANTE la aceptación
                    de términos y condiciones que completan las previsiones
                    contenidas en la presente en cuanto no se opongan a ellas.
                    LA EMPRESA no se responsabilizará por la existencia,
                    actualización, veracidad, privacidad, funcionamiento,
                    modificaciones, contenidos, ofertas y legalidad de los
                    sitios de terceros vinculados a través del presente sitio
                    web. LA EMPRESA no será responsable por las transacciones
                    efectuadas entre el USUARIO y/o VISITANTE y los sitios con
                    vínculos de la página. El USUARIO y/o VISITANTE no podrá,
                    remover, eliminar, aumentar, añadir, ni de cualquier otra
                    forma modificar total o parcialmente el Contenido. Tampoco
                    podrá volcar términos o utilizar expresiones injuriosas,
                    intimidatorias, calumniantes o contrarias a las buenas
                    costumbres. No podrá transmitir información o material que
                    pueda, concreta o eventualmente, violar derechos de un
                    tercero o que contenga virus o cualquier otro componente
                    dañino. LA EMPRESA se reserva el derecho de extraer y editar
                    en su totalidad o de manera fraccionada, cualquier mensaje o
                    material suscripto o remitido por el USUARIO y/o VISITANTE.
                    Así mismo, el USUARIO y/o VISITANTE garantiza a LA EMPRESA
                    el permiso para la utilización de cualquier información,
                    sugerencia, idea, dibujo o concepto vertido, con el
                    propósito que LA EMPRESA la utilice para la obtención de
                    información estadística que permita mejorar el servicio, sin
                    ningún derecho de compensación en favor del USUARIO y/o
                    VISITANTE. El USUARIO y/o VISITANTE se obliga a usar el
                    Sitio de conformidad con estos Términos y Condiciones, en
                    forma diligente, correcta y lícita, y conforme con la moral
                    y las buenas costumbres. El USUARIO y/o VISITANTE responderá
                    por los daños y perjuicios de toda naturaleza que LA EMPRESA
                    pueda sufrir, directa o indirectamente, como consecuencia
                    del incumplimiento de cualquiera de las obligaciones
                    derivadas de estos Términos y Condiciones. LA EMPRESA podrá
                    suspender transitoriamente o finalizar la publicación de la
                    página sin aviso previo y en cualquier momento, sin que ello
                    genere derecho a indemnización alguna en favor del USUARIO
                    y/o VISITANTE. El USUARIO y/o VISITANTE reconoce y acepta
                    que el uso de esta página es bajo su propio y exclusivo
                    riesgo. El USUARIO y/o VISITANTE reconoce y acepta que ni LA
                    EMPRESA, ni los directores, empleados o representantes de
                    cualquiera de ellos, es responsable por daños que surjan de
                    o resulten del uso de esta página, incluyendo cualquier
                    error, omisión, interrupción, falla, eliminación de archivos
                    o correos electrónicos (e-mails), defectos, virus, y/o
                    demoras en la operación o transmisión y/o de cualquier otro
                    tipo. La página puede ser utilizada por el USUARIO y/o
                    VISITANTE en forma totalmente libre y gratuita. 2. SITIOS
                    ENLAZADOS A través de la presente página se pone a su
                    disposición dispositivos técnicos de enlace (tales como,
                    entre otros, links, banners, botones), directorios y
                    herramientas de búsqueda que les permiten acceder a páginas
                    web pertenecientes a terceros (en adelante los “SITIOS
                    ENLAZADOS”). La instalación de estos enlaces en las páginas
                    de LA EMPRESA se limita a facilitar a los USUARIO y/o
                    VISITANTE, la búsqueda y acceso, a la información disponible
                    de los sitios enlazados en Internet, y no presupone que
                    existe ninguna clase de vínculo o asociación entre LA
                    EMPRESA, sus subsidiarias o afiliadas y los operadores de
                    los sitios enlazados. LA EMPRESA no controla, ni hacen
                    propios los servicios, información, datos, archivos,
                    productos y cualquier clase de material existente en los
                    sitios enlazados. Por lo tanto, el USUARIO y/o VISITANTE,
                    debe extremar la prudencia en la valoración y utilización de
                    los servicios, información, datos, archivos, productos y
                    cualquier clase de material existente en los sitios
                    enlazados. LA EMPRESA no garantiza ni asume responsabilidad
                    alguna por los daños y perjuicios que de cualquier
                    naturaleza pueda causarse por: a) El funcionamiento,
                    disponibilidad, accesibilidad o continuidad de sitios
                    enlazados. b) El mantenimiento de los servicios,
                    información, datos, archivos, productos y cualquier clase de
                    material existente en los sitios enlazados. c) Las
                    obligaciones y ofertas existentes en los sitios enlazados.
                    2.1. CALIDAD Mediante el uso de esta página, el USUARIO y/o
                    VISITANTE reconoce y acepta que LA EMPRESA no controla ni
                    garantiza la ausencia de virus en el contenido de los sitios
                    enlazados, ni la ausencia de otros elementos que pudieran
                    producir alteraciones en sus sistemas informáticos o en los
                    documentos electrónicos y archivos almacenados en los
                    archivos informáticos del USUARIO. 2.2. PROPIEDAD
                    INTELECTUAL Mediante el uso de esta página, el USUARIO y/o
                    VISITANTE reconoce y acepta que LA EMPRESA queda excluida de
                    cualquier responsabilidad que pudiera ser causada por el uso
                    no autorizado de las marcas u otros derechos de propiedad
                    intelectual de terceros o contenidos en los sitios
                    enlazados. De igual manera, las eventuales referencias que
                    se hagan en esta página a cualquier producto, servicio,
                    proceso, sitio enlazado, hipertexto o cualquier otra
                    información en la que se utilicen marcas, signos distintivos
                    o dominios, el nombre comercial o el nombre del fabricante,
                    suministrador, etc., que sean titularidad de terceros, en
                    ningún momento constituirán, ni implicarán respaldo o
                    recomendación alguna por parte de LA EMPRESA. Y en ningún
                    caso, LA EMPRESA se asignará propiedad ni responsabilidad
                    sobre los mismos. 3. POLÍTICA DE PRIVACIDAD La utilización e
                    ingreso a la presente página web de LA EMPRESA, será
                    considerado como aceptación de los términos de esta Política
                    de Privacidad por parte del USUARIO y/o VISITANTE. Los datos
                    personales que el USUARIO y/o VISITANTE brinde libre y
                    voluntariamente a LA EMPRESA, tales como nombres, correo
                    electrónico, DNI, teléfono, y/o
                    cualquier otro que voluntariamente suministre a LA EMPRESA
                    cuando ello sea necesario para brindarle un servicio
                    específico, son incluidos en archivos automatizados,
                    procesados bajo normas de estricta confidencialidad y
                    protección de datos. El USUARIO y/o VISITANTE podrá brindar
                    información con respecto a sus gustos, evaluaciones y
                    preferencias. LA EMPRESA utilizará dicha información para
                    elaborar publicidad y/o perfeccionar el servicio brindado.
                    No obstante lo anterior y en cumplimiento con las leyes
                    aplicables, LA EMPRESA coopera con las autoridades
                    gubernamentales nacionales, provinciales y municipales, e
                    internacionales en cualquier investigación en relación con
                    el contenido, ya sean personales o privadas, transmitidas a
                    LA EMPRESA a través de este Sitio. 4. TERMINACIÓN DEL ACCESO
                    LA EMPRESA podrá, en cualquier momento, terminar o suspender
                    el acceso que el USUARIO y/o VISITANTE tenga a todo o parte
                    de este sitio, sin aviso previo, y sin que ello genere
                    derecho a reclamo o indemnización alguna. Ni la terminación
                    o suspensión del acceso, ni cualquier acción o inacción del
                    USUARIO y/o VISITANTE, terminará las disposiciones de estos
                    Términos y Condiciones, los que permanecerán en plena fuerza
                    y vigor de forma indefinida, sujetas sólo a cualquier cambio
                    que LA EMPRESA efectúe. 5. VIOLACIONES DEL SISTEMA O BASES
                    DE DATOS Es ilícita cualquier acción o uso de dispositivos,
                    software, u otros instrumentos tendientes a interferir tanto
                    en las actividades y operatoria de LA EMPRESA, así como en
                    las ofertas, descripciones, cuentas o bases de datos de LA
                    EMPRESA. Cualquier intromisión, tentativa o actividad
                    violatoria o contraria a las leyes sobre derechos de
                    propiedad intelectual, seguridad de los sistemas, y/o a las
                    prohibiciones estipuladas en este documento harán pasible a
                    su responsable de las acciones legales pertinentes, y a las
                    sanciones previstas por este acuerdo. 6. JURISDICCIÓN Estos
                    Términos y Condiciones se regirán e interpretarán de acuerdo
                    con las leyes de la República Argentina. EL USUARIO y/o
                    VISITANTE conviene en que cualquier acción o procedimiento
                    legal entre LA EMPRESA y ellos, por cualquier propósito
                    respecto a estos Términos y Condiciones, se entablarán
                    exclusivamente en los Tribunales Ordinarios y Civiles de la
                    provincia de Buenos Aires. Mediante el presente Ud. deja
                    constancia que ha sido debidamente informado respecto de la
                    finalidad para la cual han sido recabados sus datos
                    personales y quiénes pueden ser sus destinatarios, como así
                    también el derecho que le asiste a tener acceso a los
                    mismos, como así a peticionar su rectificación y/o
                    supresión, todo ello conforme a lo normado en la Ley
                    Nacional de la República Argentina N° 25326 (Protección de
                    Datos Personales) y su Decreto Reglamentario N°
                    1558/01.</v-card-text
                  >
                  <v-card-actions>
                    <v-spacer></v-spacer>

                    <v-btn color="green darken-1" text @click="dialog = false"
                      >Cerrar</v-btn
                    >
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-row>

            <v-btn color="blue" text>
              Guardar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-img>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    show1: false,
    aux_next: "",
    valid: "",
    next: "NO",
    password: "Password",
    nombre_usuario: "",
    apellido: "",
    nombre: "",
    telefono: "",
    email: "",

    rules: {
      required: (value) => !!value || "Requerido.",
      min: (v) => v.length >= 8 || "Minimo 8 caracteres",
    },
  }),
  methods: {
    /*   comprobar(){
        if(this.usuario=='jorge' && this.password=='12345678'  ){
          this.next='SI';
          
        
          

        }else{
          this.next='NO',
          alert('No puede ingresar')
        }
        this.aux_next=this.next;
//this.$emit("estado_login",this.aux_next)

      },
      */
    validate() {
      if (this.$refs.form.validate()) {
        this.close();
      }
    },
  },
};
</script>
<style>
.display-1 {
  color: gray;
  text-shadow: 100px;
}
.display-2 {
  color: rgb(5, 7, 80);
  text-shadow: 20px;
}
</style>

//
src="https://imgsvr.radiocut.site/get/thumb/600/600/cuts_logos/d5/6c/d56c8368-b37f-4997-8cf6-328b2262cdcd.jpg"
