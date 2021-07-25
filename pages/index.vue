<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card>
          <v-card-title class="headline">
            Bienvenido al layout funcional de Axel Gonzalez
          </v-card-title>
          <v-card-text>
            <p>En esta aplicacion tienes ya realizado, login, registro de usuarios, recuperacion de passwords y editor de cuentas!</p>
            <p>
              Contactame por  <a
                href="https://wa.me/527221131823"
                target="_blank"
                rel="noopener noreferrer"
              >
                WhatsApp
              </a> si hay dudas.
            </p>
            <div class="text-xs-right">
              <em><small>&mdash; Edgar Axel Gonzalez Martinez</small></em>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  layout:'logged',
  data() {
    return{

    }
  },
  mounted(){
    this.$router.push('/post')
  },
}
</script>