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
          <v-parallax
            src="https://imgsvr.radiocut.site/get/thumb/600/600/cuts_logos/d5/6c/d56c8368-b37f-4997-8cf6-328b2262cdcd.jpg"
          >
            <v-row justify="center">
              <v-col class="text-center" cols="12">
                <h1 class="display-1">LOGIN</h1>
              </v-col>
            </v-row>
            <template>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="12">
                      <v-text-field
                        v-model="usuario"
                        label="Email"
                        hint=""
                        counter
                        :rules="[rules.required]"
                      ></v-text-field>
                    </v-col>
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

          <v-card-title>PERALTA JORGE EDUARDO</v-card-title>

          <v-card-subtitle class="pb-0">------</v-card-subtitle>

          <v-card-text class="text--primary">
            <div></div>

            <div></div>
          </v-card-text>

          <v-card-actions>
            <v-btn color="lime accent-3" text @click="comprobar">
              Ingresar
            </v-btn>

            <v-btn color="blue lighten-4" text href="src\App.vue">
              Registrarse
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
    show1: false,
    aux_next: "",
    valid: "",
    next: "NO",
    password: "12345678",
    usuario: "",
    rules: {
      required: (value) => !!value || "Requerido.",
      min: (v) => v.length >= 8 || "Minimo 8 caracteres",
    },
  }),
  methods: {
    comprobar() {
      if (this.usuario == "jorge" && this.password == "12345678") {
        this.next = "SI";
      } else {
        (this.next = "NO"), alert("No puede ingresar");
      }
      this.aux_next = this.next;
      this.$emit("estado_login", this.aux_next);
    },

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
