<template>
  <v-layout class="rounded rounded-md">
    <!-- Barra de aplicación con ícono de menú -->
    <v-app-bar color="white" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title>Application Bar</v-toolbar-title>
    </v-app-bar>

    <!-- Menú de navegación lateral -->
    <v-navigation-drawer v-model="drawer" app>
      <v-list dense>
        <v-list-item-group>
          <v-list-item
            v-for="item in menuItems"
            :key="item.component"
            @click="showComponent(item.component)"
          >
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <!-- Contenido principal -->
    <v-main>
      <!-- Indicador de carga circular -->
      <v-progress-circular
        v-if="loading"
        indeterminate
        color="primary"
        size="64"
        class="ma-4"
      ></v-progress-circular>

      <!-- Indicador de carga lineal -->
      <v-progress-linear
        v-if="loading"
        indeterminate
        color="primary"
        class="mb-4"
      ></v-progress-linear>

      <!-- Alerta de error -->
      <v-alert v-if="error" type="error" dismissible class="ma-4">
        {{ error }}
      </v-alert>

      <!-- Componentes -->
      <Imagen v-if="currentComponent === 'home' && !loading && !error" />
      <PeliculasLista
        v-if="currentComponent === 'peliculas' && !loading && !error"
      />
      <div v-if="currentComponent === 'contact' && !loading && !error">
        Contact Page
      </div>
    </v-main>
  </v-layout>
</template>

<script>
import PeliculasLista from "@/components/PeliculasLista.vue";
import Imagen from "@/components/imagen.vue";

export default {
  components: {
    PeliculasLista,
    Imagen,
  },
  data() {
    return {
      drawer: false,
      currentComponent: "home",
      loading: false,
      error: null,
      menuItems: [
        { title: "Home", component: "home" },
        { title: "Peliculas", component: "peliculas" },
        { title: "Contact", component: "contact" },
      ],
    };
  },
  methods: {
    async showComponent(component) {
      this.drawer = false;
      this.loading = true;
      this.error = null;

      try {
        // Simulación de carga de datos
        await new Promise((resolve) => setTimeout(resolve, 1500));
        this.currentComponent = component;
      } catch (err) {
        this.error =
          "Error al cargar el componente. Por favor, inténtelo de nuevo.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.v-application {
  background-color: white !important;
}
</style>
