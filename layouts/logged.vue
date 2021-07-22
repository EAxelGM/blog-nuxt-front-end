<template>
  <v-app style="background-color: #ababab;">
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="false"
      :clipped="false"
      fixed
      app
    >
      <v-list>
        <v-list-item>
          <v-list-item-avatar>
            <v-img :src="$auth.user.image ? $auth.user.image : 'http://localhost:8000/default/avatar.jpg'" />
          </v-list-item-avatar>
        </v-list-item>

        <v-list-item link @click="openSettings = !openSettings">
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              {{$auth.user.name}} {{$auth.user.lastname}}
            </v-list-item-title>
            <v-list-item-subtitle>{{$auth.user.email}}</v-list-item-subtitle>
          </v-list-item-content>

          <v-list-item-action>
            <v-icon>{{ openSettings ? 'mdi-menu-up' : 'mdi-menu-down'}}</v-icon>
          </v-list-item-action>
        </v-list-item>
      </v-list>
      <v-divider />
      <v-banner v-model="openSettings" single-line transition="slide-y-transition">
        <v-list
          nav
          dense
          transition="scale-transition"
        >
          <v-list-item-group
            color="primary"
          >
            <v-list-item
              v-for="(item, i) in itemsSettingsAccount"
              :key="i"
              :to="item.to"
              router
              exact
            >
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon>

              <v-list-item-content>
                <v-list-item-title v-text="item.text"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-banner>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>Blog - {{$auth.user.name}} {{$auth.user.lastname}}</v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="true"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
    <Alerta />
  </v-app>
</template>

<script>
import Alerta from '@/components/Alertas/Alerta'
export default {
  middleware: 'auth',
  components:{
    Alerta,
  },
  data () {
    return {
      itemsSettingsAccount: [
        { text: 'Editar Mi Cuenta', icon: 'mdi-account-cog-outline', to: '/mi-cuenta'},
        { text: 'Modificar Contraseña', icon: 'mdi-lock-reset', to: '/mi-cuenta/modificar-password' },
      ],
      items: [
        {
          icon: 'mdi-apps',
          title: 'Inicio',
          to: '/'
        },
        {
          icon: 'mdi-logout',
          title: 'Salir del sistema',
          to: '/loggout'
        }
      ],

      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
      openSettings: false,
    }
  }
}
</script>
