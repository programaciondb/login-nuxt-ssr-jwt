<template>
  <v-app dark>
    <v-navigation-drawer
     v-if="auth"
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
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
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon v-if="auth" @click.stop="drawer = !drawer" />
      <v-btn v-if="auth" icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? "right" : "left"}` }}</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <template v-if="auth">
        <v-btn color="success" @click="logout">Logout</v-btn>
      </template>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: true,
      fixed: false,
      items: [
        {
          icon: "mdi-room-service",
          title: "Reservas",
          to: "/reservations",
        },
        {
          icon: "mdi-account",
          title: "Clientes",
          to: "/clients",
        },
        {
          icon: "mdi-credit-card-outline",
          title: "Pagar",
          to: "/createtx",
        },
        {
          icon: "mdi-poll",
          title: "Transbank",
          to: "/",
        },
        {
          icon: "mdi-chart-bar-stacked",
          title: "Paypal",
          to: "/paypaltx",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Administración de Pagos",
    };
  },
  computed: {
    auth: function () {
      return this.$auth.loggedIn;
    }
  },
  methods: {
    async logout() {
      await this.$auth.logout();
      await this.$router.push('/login');
    }
  }
};
</script>
<style scoped>
.theme--dark.v-application {
  background-color: var(--v-background-base, #101651) !important;
}
/* .theme--light.v-application {
  background-color: var(--v-background-base, rgb(41, 137, 172)) !important;
} */
</style>
